# MODULE 11 - RPM - INSTALLATION - PROVISIONING - CONFIGURATION 
## Grafana, Prometheus, Loki, Alloy, rsyslog, and syslog-ng on Rocky Linux 9

### Part 5

## Recommended design

Use one Rocky Linux 9 VM in Xen Orchestra as the central monitoring server:

```text
Linux servers
├── node_exporter ───────────────► Prometheus ──► Grafana dashboards
│        CPU, RAM, disk, network
│
└── rsyslog or syslog-ng ────────► Grafana Alloy ──► Loki ──► Grafana logs
         operating-system logs
```

The important distinction is:

- **Prometheus + Node Exporter = metrics**
- **Loki + Alloy + rsyslog/syslog-ng = logs**
- **Grafana = visualization**
- **Alloy = log collection and forwarding into Loki**

For a new installation, use **Grafana Alloy**, not Promtail. Promtail reached end of life on March 2, 2026, and future development moved to Alloy.

Your VM can run this as a classroom demonstration:

```text
CPU:     1 vCPU
RAM:     4 GB
Disk:    15 GB
OS:      Rocky Linux 9
```

However, 15 GB is tight. The configuration below limits Loki logs to **24 hours** and Prometheus metrics to **3 days or 2 GB**, whichever is reached first.

For monitoring more than a few systems, resize later to at least:

```text
2 vCPU
8 GB RAM
50–100 GB disk
```

---

# Phase 1: Prepare the monitoring VM

## 1. Give the VM a static IP

This is critical because Xen Orchestra VM addresses can change after restarts.

For this guide, the following example address is used:

```text
Monitoring server IP: 192.168.1.200
Network:              192.168.1.0/24
Gateway:              192.168.1.1
```

Replace `192.168.1.200` with the real unused static IP you choose.

Check your interface and current address:

```bash
nmcli connection show
ip -4 address
ip route
```

Confirm that the IP is not already in use:

```bash
arping -D -I enX0 192.168.1.200 -c 5
```

Zero replies normally means that no other machine is answering for that address.

---

## 2. Update Rocky Linux

```bash
sudo dnf update -y
sudo reboot
```

After reconnecting:

```bash
hostnamectl
free -h
df -h
```

---

# Phase 2: Install Docker Engine

## 3. Remove conflicting packages

On a fresh monitoring VM:

```bash
sudo dnf remove -y \
    docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine \
    podman \
    runc
```

It is normal if DNF reports that some packages are not installed.

Do not remove Podman if this VM is already using it for something important.

---

## 4. Install Docker

```bash
sudo dnf install -y dnf-plugins-core curl tar openssl
```

Add Docker's RHEL repository:

```bash
sudo dnf config-manager --add-repo \
https://download.docker.com/linux/rhel/docker-ce.repo
```

Install Docker:

```bash
sudo dnf install -y \
docker-ce \
docker-ce-cli \
containerd.io \
docker-buildx-plugin \
docker-compose-plugin
```

Start and enable Docker:

```bash
sudo systemctl enable --now docker
```

Verify:

```bash
sudo docker run --rm hello-world
sudo docker compose version
sudo systemctl status docker --no-pager
```

---

# Phase 3: Protect the small disk

Docker container JSON logs can consume the disk independently of Loki.

## 5. Configure Docker log rotation

Create the Docker daemon configuration:

```bash
sudo tee /etc/docker/daemon.json > /dev/null <<'EOF'
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
EOF
```

Validate the JSON:

```bash
python3 -m json.tool /etc/docker/daemon.json
```

Restart Docker:

```bash
sudo systemctl restart docker
```

---

# Phase 4: Create the observability stack

## 6. Create the directories

```bash
sudo mkdir -p /opt/observability/{prometheus,loki,alloy}
sudo mkdir -p /opt/observability/grafana/provisioning/datasources

cd /opt/observability
```

---

## 7. Create the environment file

Replace `192.168.1.200` with your monitoring VM address:

```bash
sudo tee /opt/observability/.env > /dev/null <<EOF
MONITOR_IP=192.168.1.200
GRAFANA_ADMIN_PASSWORD=$(openssl rand -hex 16)
EOF
```

Protect it:

```bash
sudo chmod 600 /opt/observability/.env
sudo cat /opt/observability/.env
```

Write down the generated Grafana password.

---

## 8. Create the Prometheus configuration

```bash
sudo tee /opt/observability/prometheus/prometheus.yml > /dev/null <<'EOF'
global:
  scrape_interval: 30s
  evaluation_interval: 30s

storage:
  tsdb:
    retention:
      time: 3d
      size: 2GB

scrape_configs:
  # Monitor Prometheus itself
  - job_name: prometheus
    static_configs:
      - targets:
          - prometheus:9090

  # Monitor the Rocky Linux VM running this Docker stack
  - job_name: monitoring-server
    static_configs:
      - targets:
          - node-exporter:9100
        labels:
          server_role: monitoring

  # Add remote Linux machines here later
  - job_name: linux-servers
    static_configs:
      # Example:
      # - targets:
      #     - 192.168.1.184:9100
      #   labels:
      #     hostname: student-server-01
      #
      # - targets:
      #     - 192.168.1.185:9100
      #   labels:
      #     hostname: student-server-02
      - targets: []
EOF
```

Prometheus uses Node Exporter to expose Linux hardware and kernel metrics. Prometheus then scrapes the Node Exporter HTTP endpoint.

---

## 9. Create the Loki configuration

```bash
sudo tee /opt/observability/loki/loki.yml > /dev/null <<'EOF'
auth_enabled: false

server:
  http_listen_port: 3100
  grpc_listen_port: 9096
  log_level: info

common:
  instance_addr: 127.0.0.1
  path_prefix: /loki

  storage:
    filesystem:
      chunks_directory: /loki/chunks
      rules_directory: /loki/rules

  replication_factor: 1

  ring:
    kvstore:
      store: inmemory

schema_config:
  configs:
    - from: 2024-01-01
      store: tsdb
      object_store: filesystem
      schema: v13
      index:
        prefix: index_
        period: 24h

limits_config:
  allow_structured_metadata: true
  volume_enabled: true
  retention_period: 24h

compactor:
  working_directory: /loki/retention
  compaction_interval: 10m
  retention_enabled: true
  retention_delete_delay: 1h
  delete_request_store: filesystem

analytics:
  reporting_enabled: false
EOF
```

This runs Loki in single-binary mode with filesystem storage, which is suitable for evaluation, testing, and classroom environments.

---

## 10. Create the Grafana Alloy configuration

This configuration makes Alloy listen for RFC 5424 syslog messages on:

```text
TCP 1514
UDP 1514
```

It sends those messages to Loki.

```bash
tee /opt/observability/alloy/config.alloy > /dev/null <<'EOF'
logging {
  level  = "info"
  format = "logfmt"
}

// Define rules that convert Alloy's internal syslog labels
// into permanent Loki labels.
loki.relabel "syslog_labels" {
  // Required because forward_to is mandatory for loki.relabel.
  // This component is being used to export its rules.
  forward_to = []

  rule {
    source_labels = ["__syslog_message_hostname"]
    target_label  = "host"
  }

  rule {
    source_labels = ["__syslog_message_app_name"]
    target_label  = "app"
  }

  rule {
    source_labels = ["__syslog_message_severity"]
    target_label  = "severity"
  }

  rule {
    source_labels = ["__syslog_message_facility"]
    target_label  = "facility"
  }

  rule {
    source_labels = ["__syslog_connection_ip_address"]
    target_label  = "source_ip"
  }
}

// Receive remote Linux syslog messages over TCP and UDP.
loki.source.syslog "remote_linux_logs" {
  listener {
    address       = "0.0.0.0:1514"
    protocol      = "tcp"
    syslog_format = "rfc5424"

    labels = {
      job       = "syslog",
      transport = "tcp",
    }
  }

  listener {
    address        = "0.0.0.0:1514"
    protocol       = "udp"
    syslog_format  = "rfc5424"
    udp_queue_size = 100

    labels = {
      job       = "syslog",
      transport = "udp",
    }
  }

  // Apply the rules before internal __syslog_* labels are removed.
  relabel_rules = loki.relabel.syslog_labels.rules

  // Forward the resulting logs to Loki.
  forward_to = [loki.write.local_loki.receiver]
}

// Send received logs to the Loki container.
loki.write "local_loki" {
  endpoint {
    url = "http://loki:3100/loki/api/v1/push"
  }
}

// Enable Alloy live debugging in the Alloy web interface.
livedebugging {
  enabled = true
}
EOF
```

---

## 11. Provision the Grafana data sources

This automatically adds Prometheus and Loki when Grafana starts.

```bash
tee \
/opt/observability/grafana/provisioning/datasources/datasources.yml \
> /dev/null <<'EOF'
apiVersion: 1

datasources:
  - name: Prometheus
    uid: prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
    isDefault: true
    editable: true

  - name: Loki
    uid: loki
    type: loki
    access: proxy
    url: http://loki:3100
    editable: true
    jsonData:
      maxLines: 1000
EOF
```

---

## 12. Create Docker Compose

```bash
sudo tee /opt/observability/docker-compose.yml > /dev/null <<'EOF'
name: nit-observability

services:
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped

    ports:
      - "${MONITOR_IP}:3000:3000"

    environment:
      GF_SECURITY_ADMIN_USER: admin
      GF_SECURITY_ADMIN_PASSWORD: "${GRAFANA_ADMIN_PASSWORD}"
      GF_USERS_ALLOW_SIGN_UP: "false"

    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana/provisioning:/etc/grafana/provisioning:ro,Z

    mem_limit: 512m

    depends_on:
      - prometheus
      - loki

    networks:
      - monitoring

  prometheus:
    image: prom/prometheus:v3.13.1
    container_name: prometheus
    restart: unless-stopped

    ports:
      - "${MONITOR_IP}:9090:9090"

    command:
      - "--config.file=/etc/prometheus/prometheus.yml"
      - "--storage.tsdb.path=/prometheus"
      - "--web.enable-lifecycle"

    volumes:
      - prometheus_data:/prometheus
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro,Z

    mem_limit: 768m

    networks:
      - monitoring

  loki:
    image: grafana/loki:3.7.3
    container_name: loki
    restart: unless-stopped

    ports:
      - "127.0.0.1:3100:3100"

    command:
      - "-config.file=/etc/loki/loki.yml"

    volumes:
      - loki_data:/loki
      - ./loki/loki.yml:/etc/loki/loki.yml:ro,Z

    mem_limit: 1g

    networks:
      - monitoring

  alloy:
    image: grafana/alloy:latest
    container_name: alloy
    restart: unless-stopped

    ports:
      - "${MONITOR_IP}:12345:12345"
      - "${MONITOR_IP}:1514:1514/tcp"
      - "${MONITOR_IP}:1514:1514/udp"

    command:
      - run
      - "--server.http.listen-addr=0.0.0.0:12345"
      - "--storage.path=/var/lib/alloy/data"
      - "/etc/alloy/config.alloy"

    volumes:
      - alloy_data:/var/lib/alloy/data
      - ./alloy/config.alloy:/etc/alloy/config.alloy:ro,Z

    mem_limit: 384m

    depends_on:
      - loki

    networks:
      - monitoring

  node-exporter:
    image: prom/node-exporter:v1.10.2
    container_name: node-exporter
    restart: unless-stopped

    command:
      - "--path.procfs=/host/proc"
      - "--path.sysfs=/host/sys"
      - "--path.rootfs=/rootfs"
      - "--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)"

    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro

    security_opt:
      - label=disable

    expose:
      - "9100"

    mem_limit: 128m

    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge

volumes:
  grafana_data:
  prometheus_data:
  loki_data:
  alloy_data:
EOF
```

---

# Phase 5: Validate and start the stack

## 13. Check the Compose file

```bash
cd /opt/observability

sudo docker compose config
```

Validate the Alloy configuration:

```bash
sudo docker compose run --rm --no-deps \
alloy validate /etc/alloy/config.alloy
```

Expected result:

```text
Configuration loaded successfully
```
```bash
docker compose run --rm --no-deps \
  alloy validate /etc/alloy/config.alloy \
  && echo "ALLOY CONFIGURATION IS VALID" \
  || echo "ALLOY CONFIGURATION FAILED"
```
---

## 14. Pull and start the containers

```bash
sudo docker compose pull
sudo docker compose up -d
```

Check status:

```bash
sudo docker compose ps
```

You should see:

```text
grafana         Up
prometheus      Up
loki            Up
alloy           Up
node-exporter   Up
```

Check logs:

```bash
sudo docker compose logs --tail=50 grafana
sudo docker compose logs --tail=50 prometheus
sudo docker compose logs --tail=50 loki
sudo docker compose logs --tail=50 alloy
sudo docker compose logs --tail=50 node-exporter
```

Validate Prometheus:

```bash
sudo docker exec prometheus \
promtool check config /etc/prometheus/prometheus.yml
```

Test Loki:

```bash
curl http://127.0.0.1:3100/ready
```

Expected:

```text
ready
```

---

# Phase 6: Open the classroom ports

## 15. Configure firewalld

```bash
sudo firewall-cmd --permanent --add-port=3000/tcp
sudo firewall-cmd --permanent --add-port=9090/tcp
sudo firewall-cmd --permanent --add-port=12345/tcp
sudo firewall-cmd --permanent --add-port=1514/tcp
sudo firewall-cmd --permanent --add-port=1514/udp

sudo firewall-cmd --reload
sudo firewall-cmd --list-ports
```

Do **not** port-forward these ports on your Verizon router. Keep them accessible only from your internal network or VPN.

The ports are:

| Port | Purpose |
|---:|---|
| 3000/TCP | Grafana |
| 9090/TCP | Prometheus UI |
| 12345/TCP | Alloy debugging UI |
| 1514/TCP | Reliable syslog forwarding |
| 1514/UDP | Optional UDP syslog |
| 3100/TCP | Loki, bound only to localhost |

Check listening ports:

```bash
sudo ss -lntup | grep -E '3000|9090|3100|12345|1514'
```

---

# Phase 7: Access Grafana

Open:

```text
http://192.168.1.200:3000
```

Use:

```text
Username: admin
Password: value stored in /opt/observability/.env
```

View the password:

```bash
sudo grep GRAFANA_ADMIN_PASSWORD /opt/observability/.env
```

The Prometheus and Loki data sources should already be present under:

```text
Connections → Data sources
```

---

## 16. Import the Linux dashboard

In Grafana:

1. Select **Dashboards**.
2. Select **New**.
3. Select **Import**.
4. Enter dashboard ID:

```text
1860
```

5. Select the Prometheus data source.
6. Select **Import**.

Dashboard `1860` is commonly called **Node Exporter Full**.

---

## 17. Test basic Prometheus queries

Open:

```text
Explore → Prometheus
```

Check whether all targets are available:

```promql
up
```

CPU utilization:

```promql
100 - (
  avg by(instance) (
    rate(node_cpu_seconds_total{mode="idle"}[5m])
  ) * 100
)
```

Memory utilization:

```promql
100 * (
  1 -
  node_memory_MemAvailable_bytes
  /
  node_memory_MemTotal_bytes
)
```

Root filesystem usage:

```promql
100 * (
  1 -
  node_filesystem_avail_bytes{mountpoint="/"}
  /
  node_filesystem_size_bytes{mountpoint="/"}
)
```

Prometheus targets can also be inspected at:

```text
http://192.168.1.200:9090/targets
```

---

# Phase 8: Install Node Exporter on another Rocky Linux machine

Install Node Exporter on each Linux VM whose metrics you want to collect.

## 18. Create the Node Exporter account

On the client machine:

```bash
sudo useradd \
--system \
--no-create-home \
--shell /sbin/nologin \
node_exporter
```

---

## 19. Download Node Exporter

```bash
cd /tmp

curl -LO \
https://github.com/prometheus/node_exporter/releases/download/v1.10.2/node_exporter-1.10.2.linux-amd64.tar.gz
```

Extract it:

```bash
tar -xzf node_exporter-1.10.2.linux-amd64.tar.gz
```

Install the binary:

```bash
sudo install -m 0755 \
node_exporter-1.10.2.linux-amd64/node_exporter \
/usr/local/bin/node_exporter
```

---

## 20. Create the systemd service

```bash
sudo tee /etc/systemd/system/node_exporter.service > /dev/null <<'EOF'
[Unit]
Description=Prometheus Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=node_exporter
Group=node_exporter
ExecStart=/usr/local/bin/node_exporter
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
EOF
```

Start it:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
```

Verify:

```bash
sudo systemctl status node_exporter --no-pager
curl http://127.0.0.1:9100/metrics | head
sudo ss -lntp | grep 9100
```

---

## 21. Allow only Prometheus to reach port 9100

Replace the source IP with your monitoring server:

```bash
sudo firewall-cmd --permanent \
--add-rich-rule='rule family="ipv4" source address="192.168.1.200/32" port port="9100" protocol="tcp" accept'

sudo firewall-cmd --reload
sudo firewall-cmd --list-rich-rules
```

Test from the monitoring VM:

```bash
curl http://CLIENT_IP:9100/metrics | head
```

Example:

```bash
curl http://192.168.1.184:9100/metrics | head
```

---

## 22. Add the client to Prometheus

On the monitoring server:

```bash
sudo vi /opt/observability/prometheus/prometheus.yml
```

Change the `linux-servers` section:

```yaml
  - job_name: linux-servers
    static_configs:
      - targets:
          - 192.168.1.184:9100
        labels:
          hostname: rocky-client-01
          environment: classroom

      - targets:
          - 192.168.1.185:9100
        labels:
          hostname: rocky-client-02
          environment: classroom
```

Validate:

```bash
sudo docker exec prometheus \
promtool check config /etc/prometheus/prometheus.yml
```

Reload Prometheus without restarting it:

```bash
source /opt/observability/.env

curl -X POST \
"http://${MONITOR_IP}:9090/-/reload"
```

Open:

```text
http://192.168.1.200:9090/targets
```

The new server should show:

```text
State: UP
```

---

# Phase 9: Forward logs using rsyslog

Rocky Linux normally uses rsyslog and systemd-journald for operating-system logging.

Start with rsyslog because it is normally already installed. We will select a new Virtual Machine (VM) whose logs we will send to Graphana.

The following was added as a comment by Nadeem Siddiqi on 7/19/2026
###  Select/Creating a New Linux VM
# Configure a Static IP Address Using `nmcli`

## Network Configuration

| Setting | Value |
|---|---|
| Network | `192.168.1.0/24` |
| Static IP address | `192.168.1.215/24` |
| Gateway | `192.168.1.1` |
| Network interface | `enX0` |
| DNS servers | `192.168.1.1` and `8.8.8.8` |

## 1. Check Whether the IP Address Is Available

Before assigning the IP address, check whether `192.168.1.215` is already being used by another device:

```bash
arping -D -I enX0 192.168.1.215 -c 5
```

The following result normally indicates that the IP address is available:

```text
Received 0 response(s)
```

If responses are received, another device may already be using the IP address. Do not assign it until the conflict has been resolved.

## 2. Configure the Static IP Address

Run the following command:

```bash
nmcli connection modify enX0 \
  ipv4.method manual \
  ipv4.addresses 192.168.1.215/24 \
  ipv4.gateway 192.168.1.1 \
  ipv4.dns "192.168.1.1 8.8.8.8"
```

## 3. Apply the Configuration

Restart the NetworkManager connection:

```bash
nmcli connection down enX0
nmcli connection up enX0
```

> **Warning:** If you are connected to the server through SSH, your SSH session may disconnect when the network connection is restarted.

Reconnect using the new static IP address:

```bash
ssh root@192.168.1.215
```

## 4. Verify the IP Address

Check the IPv4 address assigned to `enX0`:

```bash
ip -4 address show enX0
```

Check the routing table:

```bash
ip route
```

Check the IP address, gateway, and DNS settings:

```bash
nmcli device show enX0 | grep -E 'IP4.ADDRESS|IP4.GATEWAY|IP4.DNS'
```

The expected default route should look similar to:

```text
default via 192.168.1.1 dev enX0
```

The interface should show the following IP address:

```text
192.168.1.215/24
```

## 5. Test Network Connectivity

Test connectivity to the gateway:

```bash
ping -c 4 192.168.1.1
```

Test internet connectivity using an IP address:

```bash
ping -c 4 8.8.8.8
```

Test DNS name resolution:

```bash
ping -c 4 google.com
```

If the gateway and `8.8.8.8` respond but `google.com` does not, the issue is most likely related to DNS configuration.
---
# 23a. Configuring the New Linux VM
## Prepare VM2 to Forward rsyslog Logs to the Monitoring Server

## Environment

This guide prepares a second Rocky Linux 9 VM to forward its operating-system logs to the monitoring server.

```text
Monitoring server: 192.168.1.176
Syslog receiver:   Grafana Alloy
Syslog port:       TCP 1514
Log storage:       Loki
Visualization:     Grafana
```

VM2 needs only:

- Rocky Linux 9
- rsyslog
- Network connectivity to `192.168.1.176`
- TCP access to port `1514`

You do **not** need to install Grafana, Loki, Prometheus, or Alloy on VM2.

```text
VM2 rsyslog
    │
    │ TCP 1514
    ▼
192.168.1.176 Alloy
    │
    ▼
Loki
    │
    ▼
Grafana
```

Run the following commands on **VM2**, unless a step specifically says to run it on the monitoring server.

---

# Step 1: Identify VM2

Log into VM2 and become root:

```bash
sudo -i
```

Check the operating system:

```bash
cat /etc/rocky-release
```

Check its current hostname and IP address:

```bash
hostnamectl
ip -br address
ip route
```

Give VM2 a recognizable hostname:

```bash
hostnamectl set-hostname rocky-client-01
```

Confirm:

```bash
hostname
```

Expected:

```text
rocky-client-01
```

The hostname is important because Loki should create a label similar to:

```text
host="rocky-client-01"
```

---

# Step 2: Verify Network Connectivity

From VM2:

```bash
ping -c 4 192.168.1.176
```

You should receive replies.

If ping does not work, check:

```bash
ip route
nmcli device status
```

---

# Step 3: Check Time Synchronization

Correct timestamps are important when searching logs in Grafana.

Check VM2's time:

```bash
timedatectl
```

Make sure chrony is installed and running:

```bash
dnf install -y chrony
systemctl enable --now chronyd
```

Verify:

```bash
chronyc tracking
```

The times on VM2 and `192.168.1.176` should be reasonably close.

---

# Step 4: Install rsyslog and Connectivity Tools

On VM2:

```bash
dnf install -y rsyslog nmap-ncat
```

Enable and start rsyslog:

```bash
systemctl enable --now rsyslog
```

Check its status:

```bash
systemctl status rsyslog --no-pager
```

Expected:

```text
Active: active (running)
```

Verify the process:

```bash
ps -ef | grep '[r]syslog'
```

---

# Step 5: Test TCP Port 1514

From VM2:

```bash
nc -vz 192.168.1.176 1514
```

Expected:

```text
Ncat: Connected to 192.168.1.176:1514.
```

This confirms that:

- VM2 can reach the monitoring server.
- The firewall permits TCP port 1514.
- Alloy or Docker is listening on port 1514.

## If the connection fails

Run these commands on the monitoring server, `192.168.1.176`:

```bash
cd /opt/observability
sudo docker compose ps
sudo ss -lntup | grep 1514
sudo firewall-cmd --list-ports
```

You should see:

```text
1514/tcp
1514/udp
```

Add TCP port 1514 if necessary:

```bash
sudo firewall-cmd --permanent --add-port=1514/tcp
sudo firewall-cmd --reload
```

Return to VM2 and test again:

```bash
nc -vz 192.168.1.176 1514
```

> VM2 does not normally need an inbound firewall port opened. It makes an outbound connection to the monitoring server.

---

# Step 6: Check for Existing Forwarding Configurations

Before creating the new file, check whether VM2 is already forwarding logs somewhere:

```bash
grep -RniE 'omfwd|@@|@192\.168|target=' \
/etc/rsyslog.conf /etc/rsyslog.d/ 2>/dev/null
```

If no existing forwarding configuration appears, continue.

If an older configuration points elsewhere, disable it to prevent duplicate forwarding.

---

# Step 7: Create the rsyslog Forwarding Configuration

Create the configuration:

```bash
tee /etc/rsyslog.d/90-grafana-alloy.conf > /dev/null <<'EOF'
# Forward all logs to Grafana Alloy on the monitoring server.
# TCP is used because it is more reliable than UDP.

action(
    type="omfwd"
    target="192.168.1.176"
    port="1514"
    protocol="tcp"
    template="RSYSLOG_SyslogProtocol23Format"

    action.resumeRetryCount="-1"

    queue.type="linkedList"
    queue.size="10000"
)
EOF
```

Display the file:

```bash
cat /etc/rsyslog.d/90-grafana-alloy.conf
```

| Setting | Meaning |
|---|---|
| `target` | Monitoring server IP |
| `port` | Alloy syslog listening port |
| `protocol="tcp"` | Reliable TCP transport |
| `RSYSLOG_SyslogProtocol23Format` | Sends RFC 5424-formatted logs |
| `resumeRetryCount="-1"` | Continue retrying if the server is unavailable |
| `queue.type="linkedList"` | Queue messages instead of blocking local logging |
| `queue.size="10000"` | Queue capacity |

---

# Step 8: Validate the rsyslog Configuration

```bash
rsyslogd -N1
```

A successful validation should end with:

```text
rsyslogd: End of config validation run. Bye.
```

Do not restart rsyslog until validation succeeds.

---

# Step 9: Restart rsyslog

```bash
systemctl restart rsyslog
systemctl status rsyslog --no-pager
```

Check recent service messages:

```bash
journalctl -u rsyslog --since "5 minutes ago" --no-pager
```

Check for errors:

```bash
journalctl -u rsyslog -p err --since "10 minutes ago" --no-pager
```

---

# Step 10: Generate the First Test Log

```bash
logger -p user.notice -t nit-test \
"NIT rsyslog test from $(hostname) at $(date)"
```

Verify it locally:

```bash
journalctl -t nit-test -n 10 --no-pager
```

You may also find it in `/var/log/messages`:

```bash
grep "NIT rsyslog test" /var/log/messages
```

---

# Step 11: Generate Additional Test Logs

```bash
logger -p user.info -t classroom \
"NIT classroom information message from $(hostname)"

logger -p user.warning -t classroom \
"NIT classroom warning message from $(hostname)"

logger -p user.err -t classroom \
"NIT classroom error message from $(hostname)"

logger -p authpriv.notice -t sshd \
"NIT classroom simulated SSH log from $(hostname)"
```

Check them locally:

```bash
journalctl --since "5 minutes ago" | grep NIT
```

---

# Step 12: Watch Traffic on VM2

Install tcpdump:

```bash
dnf install -y tcpdump
```

In one VM2 terminal:

```bash
tcpdump -ni any host 192.168.1.176 and port 1514
```

From another VM2 terminal:

```bash
logger -t nit-test \
"NIT tcpdump forwarding test from $(hostname)"
```

Press `Ctrl+C` to stop tcpdump.

---

# Step 13: Verify Traffic on the Monitoring Server

On `192.168.1.176`:

```bash
sudo tcpdump -ni any port 1514
```

Then send another message from VM2:

```bash
logger -t nit-test \
"NIT incoming traffic test from $(hostname)"
```

---

# Step 14: Check the Alloy Container

On `192.168.1.176`:

```bash
cd /opt/observability
sudo docker compose ps
sudo docker compose logs --tail=100 alloy
```

Follow Alloy logs live:

```bash
sudo docker compose logs -f alloy
```

Then send another message from VM2:

```bash
logger -t nit-test \
"NIT Alloy live test from $(hostname)"
```

> Alloy may not print every individual message at the default `info` logging level. Network traffic and Grafana queries are better confirmation methods.

---

# View VM2 Logs in Grafana

Open:

```text
http://192.168.1.176:3000
```

Go to:

```text
Explore → Loki
```

Set the time range to:

```text
Last 15 minutes
```

## Display all syslog messages

```logql
{job="syslog"}
```

## Find the VM2 test message

```logql
{job="syslog"} |= "NIT rsyslog test"
```

## Filter by VM2 hostname

```logql
{job="syslog", host="rocky-client-01"}
```

## Filter by source IP

```logql
{job="syslog", source_ip="VM2_IP_ADDRESS"}
```

Example:

```logql
{job="syslog", source_ip="192.168.1.184"}
```

Find VM2's IP:

```bash
hostname -I
```

## Find the simulated SSH message

```logql
{job="syslog"} |= "NIT classroom simulated SSH log"
```

## Find real SSH failures

```logql
{job="syslog"} |= "Failed password"
```

## Filter by application

```logql
{job="syslog", app="classroom"}
```

```logql
{job="syslog", app="nit-test"}
```

## Filter warning logs

```logql
{job="syslog", severity="warning"}
```

## Count logs per host

```logql
sum by (host) (
  count_over_time({job="syslog"}[5m])
)
```

Count only VM2:

```logql
sum(
  count_over_time(
    {job="syslog", host="rocky-client-01"}[5m]
  )
)
```

---

# Complete VM2 Test Block

```bash
logger -p user.notice -t nit-test \
"NIT rsyslog test from $(hostname) at $(date)"

logger -p user.info -t classroom \
"NIT classroom information message from $(hostname)"

logger -p user.warning -t classroom \
"NIT classroom warning message from $(hostname)"

logger -p user.err -t classroom \
"NIT classroom error message from $(hostname)"

logger -p authpriv.notice -t sshd \
"NIT classroom simulated SSH log from $(hostname)"
```

Verify locally:

```bash
journalctl --since "5 minutes ago" | grep NIT
```

Then query in Grafana:

```logql
{job="syslog"} |= "NIT"
```

---

# Troubleshooting

## `nc` reports connection refused

On `192.168.1.176`:

```bash
cd /opt/observability
sudo docker compose ps alloy
sudo ss -lntup | grep 1514
sudo firewall-cmd --list-ports
sudo docker compose restart alloy
sudo docker compose logs --tail=100 alloy
```

## rsyslog validation fails

```bash
cat -n /etc/rsyslog.d/90-grafana-alloy.conf
rsyslogd -N1
```

## Test exists locally but not in Grafana

```bash
journalctl -t nit-test -n 20 --no-pager
nc -vz 192.168.1.176 1514
tcpdump -ni any host 192.168.1.176 and port 1514
systemctl restart rsyslog
```

Generate a unique test:

```bash
logger -t nit-test \
"UNIQUE-VM2-TEST-$(date +%s)"
```

Search in Grafana:

```logql
{job="syslog"} |= "UNIQUE-VM2-TEST"
```

## Host label is empty

```bash
hostname
hostnamectl --static
systemctl restart rsyslog
```

Then send:

```bash
logger -t nit-test \
"NIT hostname label test from $(hostname)"
```

---

# Final Validation Checklist

On VM2:

```bash
hostname
systemctl is-active rsyslog
rsyslogd -N1
nc -vz 192.168.1.176 1514
```

Generate the final test:

```bash
logger -p user.notice -t nit-test \
"NIT FINAL VM2 TEST from $(hostname) at $(date)"
```

In Grafana:

```logql
{job="syslog"} |= "NIT FINAL VM2 TEST"
```

Once this message appears, VM2 is successfully forwarding logs through:

```text
VM2 rsyslog
   → 192.168.1.176:1514
   → Grafana Alloy
   → Loki
   → Grafana
```
---
## 23b. Ensure rsyslog is running

On the client:

```bash
sudo dnf install -y rsyslog
sudo systemctl enable --now rsyslog
sudo systemctl status rsyslog --no-pager
```

---

## 24. Configure reliable TCP forwarding

```bash
sudo tee /etc/rsyslog.d/90-grafana-alloy.conf > /dev/null <<'EOF'
action(
    type="omfwd"
    target="192.168.1.200"
    port="1514"
    protocol="tcp"
    template="RSYSLOG_SyslogProtocol23Format"

    action.resumeRetryCount="-1"

    queue.type="linkedList"
    queue.size="10000"
)
EOF
```

The `RSYSLOG_SyslogProtocol23Format` template sends RFC 5424-formatted messages that match the Alloy listener.

Validate the configuration:

```bash
sudo rsyslogd -N1
```

Expected ending:

```text
rsyslogd: End of config validation run. Bye.
```

Restart:

```bash
sudo systemctl restart rsyslog
```

Generate a test message:

```bash
logger -p user.notice \
"NIT rsyslog test from $(hostname) at $(date)"
```

Generate an SSH-related test message:

```bash
logger -t sshd \
"NIT classroom simulated SSH log from $(hostname)"
```

---

# Phase 10: View rsyslog messages in Grafana

Open:

```text
Grafana → Explore → Loki
```

Show all received syslog:

```logql
{job="syslog"}
```

Filter by hostname:

```logql
{job="syslog", host="rocky-client-01"}
```

Search for your test:

```logql
{job="syslog"} |= "NIT rsyslog test"
```

Find SSH failures:

```logql
{job="syslog"} |= "Failed password"
```

Count logs per host over five minutes:

```logql
sum by (host) (
  count_over_time({job="syslog"}[5m])
)
```

This last query turns logs into a graphable count.

---

# Phase 11: Use syslog-ng as an alternative

Do not normally run rsyslog and syslog-ng simultaneously on the same student VM. They may duplicate messages or compete for logging inputs.

Use a second Rocky Linux VM for the syslog-ng demonstration.

## 25. Install syslog-ng

```bash
sudo dnf install -y epel-release
sudo dnf install -y syslog-ng
```

Stop rsyslog:

```bash
sudo systemctl disable --now rsyslog
```

Back up the configuration:

```bash
sudo cp -a \
/etc/syslog-ng/syslog-ng.conf \
/etc/syslog-ng/syslog-ng.conf.bak
```

Check the existing source name:

```bash
grep -nE '^source ' /etc/syslog-ng/syslog-ng.conf
```

On many installations it will be:

```text
source s_sys
```

Append the Alloy destination:

```bash
sudo tee -a /etc/syslog-ng/syslog-ng.conf > /dev/null <<'EOF'

# Forward Linux logs to Grafana Alloy using RFC 5424 over TCP
destination d_grafana_alloy {
    syslog(
        "192.168.1.200"
        transport("tcp")
        port(1514)
    );
};

log {
    source(s_sys);
    destination(d_grafana_alloy);
};
EOF
```

If your source has a different name, replace `s_sys` with the name shown by the earlier `grep` command.

Validate:

```bash
sudo syslog-ng -s
```

Start it:

```bash
sudo systemctl enable --now syslog-ng
sudo systemctl status syslog-ng --no-pager
```

Test:

```bash
logger -p user.notice \
"NIT syslog-ng test from $(hostname) at $(date)"
```

In Grafana:

```logql
{job="syslog"} |= "NIT syslog-ng test"
```

---

# Useful troubleshooting commands

## Container status

```bash
cd /opt/observability

sudo docker compose ps
sudo docker stats --no-stream
```

## Individual logs

```bash
sudo docker compose logs --tail=100 grafana
sudo docker compose logs --tail=100 prometheus
sudo docker compose logs --tail=100 loki
sudo docker compose logs --tail=100 alloy
```

## Check Alloy's component graph

Open:

```text
http://192.168.1.200:12345
```

Look for:

```text
loki.source.syslog.remote_linux_logs
loki.write.local_loki
```

## Check whether syslog traffic reaches the server

On the monitoring VM:

```bash
sudo dnf install -y tcpdump
sudo tcpdump -ni any port 1514
```

Then run `logger` on a client.

## Check TCP connectivity

From a client:

```bash
sudo dnf install -y nmap-ncat
nc -vz 192.168.1.200 1514
```

Expected:

```text
Ncat: Connected to 192.168.1.200:1514
```

## Check Prometheus target connectivity

From the monitoring VM:

```bash
curl http://CLIENT_IP:9100/metrics | head
```

## Check disk usage

```bash
df -h
sudo docker system df
sudo du -sh /var/lib/docker/*
```

## Check memory

```bash
free -h
sudo docker stats --no-stream
```

---

# Recommended teaching sequence

Teach this stack in four separate labs:

| Lab | Student objective |
|---|---|
| Lab 1 | Install Docker and launch Grafana |
| Lab 2 | Add Prometheus and Node Exporter |
| Lab 3 | Add Loki and Grafana Alloy |
| Lab 4 | Forward logs using rsyslog and syslog-ng |

The conceptual flow should be:

```text
Metric:
Linux kernel → Node Exporter → Prometheus → Grafana

Log:
Application/systemd → journald/rsyslog → Alloy → Loki → Grafana
```

For the first class, monitor only:

```text
1 monitoring VM
1 rsyslog client
1 syslog-ng client
```

Once that works reliably, add more student machines. Before connecting dozens of student VMs, enlarge the virtual disk and preferably increase the monitoring VM to **2 vCPU and 8 GB RAM**.
