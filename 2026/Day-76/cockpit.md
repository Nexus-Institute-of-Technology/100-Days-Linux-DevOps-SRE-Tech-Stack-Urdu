# Cockpit Installation and Startup Lab (Rocky Linux 9)

## Objective

In this lab you will:

-   Install Cockpit
-   Start the Cockpit service
-   Configure the firewall
-   Verify the service
-   Connect from a web browser

> **Note:** All students are using the **root** account on their Rocky
> Linux 9 virtual machines.

------------------------------------------------------------------------

# Lab Topology

    +-------------------+              HTTPS (Port 9090)
    | Student Computer  |  ---------------------------->
    |  Web Browser      |                               |
    +-------------------+                               |
                                                        |
                                                +----------------------+
                                                | Rocky Linux 9 VM     |
                                                | Cockpit Web Console  |
                                                | TCP Port 9090        |
                                                +----------------------+

------------------------------------------------------------------------

# Step 1 -- Install Cockpit

``` bash
dnf install -y cockpit
```

Verify installation:

``` bash
rpm -q cockpit
```

Expected output:

    cockpit-<version>.el9.x86_64

------------------------------------------------------------------------

# Step 2 -- Enable and Start Cockpit

``` bash
systemctl enable --now cockpit.socket
```

Check the status:

``` bash
systemctl status cockpit.socket
```

Expected:

    Active: active (listening)

------------------------------------------------------------------------

# Step 3 -- Allow Cockpit Through the Firewall

``` bash
firewall-cmd --permanent --add-service=cockpit
firewall-cmd --reload
```

Verify:

``` bash
firewall-cmd --list-services
```

Expected to include:

    cockpit

------------------------------------------------------------------------

# Step 4 -- Find Your Server IP Address

``` bash
hostname -I
```

or

``` bash
ip addr
```

Example:

    192.168.1.191

------------------------------------------------------------------------

# Step 5 -- Connect Using a Web Browser

Open a browser and visit:

    https://<SERVER-IP>:9090

Example:

    https://192.168.1.191:9090

Your browser will display a certificate warning because Cockpit uses a
self-signed certificate by default.

Choose:

-   Advanced
-   Continue to the website

------------------------------------------------------------------------

# Step 6 -- Log In

Since this lab uses the **root** account, log in with:

    Username: root
    Password: <your root password>

------------------------------------------------------------------------

# Verification

## Check the service

``` bash
systemctl status cockpit.socket
```

## Check port 9090

``` bash
ss -tlnp | grep 9090
```

Expected:

    LISTEN 0 4096 *:9090

## Check firewall

``` bash
firewall-cmd --list-services
```

------------------------------------------------------------------------

# Troubleshooting

## Cockpit is not running

``` bash
systemctl restart cockpit.socket
systemctl status cockpit.socket
```

## Port not listening

``` bash
ss -tln | grep 9090
```

## Firewall problem

``` bash
firewall-cmd --permanent --add-service=cockpit
firewall-cmd --reload
```

------------------------------------------------------------------------

# Validation Checklist

  Task                     Completed
  ------------------------ -----------
  Cockpit installed        ☐
  Service enabled          ☐
  Service running          ☐
  Firewall configured      ☐
  Browser connected        ☐
  Logged in successfully   ☐
  Verified port 9090       ☐

------------------------------------------------------------------------

# Challenge Exercise

1.  Explore the **Overview** page.

2.  Open **Logs** and view recent system logs.

3.  Open **Services** and locate `sshd`.

4.  Open **Networking** and identify your IP address.

5.  Open **Terminal** and execute:

    ``` bash
    hostname
    uptime
    free -h
    df -h
    ```

6.  Identify your VM's CPU, memory, and disk usage from the Cockpit
    dashboard.

------------------------------------------------------------------------

# Learning Outcomes

After completing this lab, students should be able to:

-   Install Cockpit
-   Enable system services using `systemctl`
-   Configure the firewall
-   Verify listening network ports
-   Access Linux through a secure web interface
-   Perform basic Linux administration using Cockpit
