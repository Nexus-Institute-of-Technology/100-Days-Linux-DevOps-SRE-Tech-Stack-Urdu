# NexusVentures Project 19: Rootless Rsyslog Container ko Startup Service Banana

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

User `paradise` ke liye approved rsyslog image se `logserver` naam ka rootless container banayein aur use user systemd service ke taur par khudkar start hone ke liye configure karein.

## 2. Karobari Manzar Nama

NexusVentures ek alag logging workload ke liye rootless containers aur user-level service management ka jaiza le raha hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
User-systemd commands ke liye Paradise ka direct login istemal karein. Project 20 is project par depend karta hai.

## 5. Qadam-ba-Qadam Hal

## Root ki Tayyari

### Qadam 1: Container tools install karein

```bash
dnf install -y container-tools
podman --version
```

### Qadam 2: Agar zaroorat ho to user banayein

```bash
id paradise >/dev/null 2>&1 || useradd -m paradise
passwd paradise
```

### Qadam 3: Lingering enable karein

```bash
loginctl enable-linger paradise
loginctl show-user paradise -p Linger
```

Mutawaqqa nateeja: `Linger=yes`.

## Paradise ka Direct Session

`paradise` ke taur par naya direct SSH ya console login kholain. User-systemd tests ke liye `su` par bharosa na karein.

### Qadam 4: Rootless context ki tasdeeq karein

```bash
whoami
id
podman info --format '{{.Host.Security.Rootless}}'
```

Mutawaqqa nateeja: user `paradise` aur rootless value `true`.

### Qadam 5: Approved rsyslog image pull karein

```bash
CONTAINER_IMAGE="docker.io/lendingworks/rsyslog"
podman pull "$CONTAINER_IMAGE"
podman images
```

Agar yeh image dastiyab na ho to instructor se approved replacement istemal karein.

### Qadam 6: Quadlet definition banayein

```bash
mkdir -p ~/.config/containers/systemd
cat > ~/.config/containers/systemd/logserver.container <<EOF
[Unit]
Description=NexusVentures Rootless Rsyslog Container

[Container]
Image=${CONTAINER_IMAGE}
ContainerName=logserver

[Service]
Restart=always
TimeoutStartSec=180

[Install]
WantedBy=default.target
EOF
```

### Qadam 7: User unit generate aur start karein

```bash
systemctl --user daemon-reload
systemctl --user start logserver.service
systemctl --user status logserver.service --no-pager
```

### Qadam 8: Jaiza lein

```bash
podman ps -a
podman inspect logserver   --format 'Name={{.Name}} Image={{.ImageName}} Status={{.State.Status}}'
```

Agar container exit ho jaye to:

```bash
journalctl --user -u logserver.service -n 50 --no-pager
podman logs logserver
```

Instructor ko image-specific arguments dene ki zaroorat par sakti hai.

### Qadam 9: Host reboot karein

Root ke taur par:

```bash
reboot
```

Reboot ke baad Paradise ke taur par direct login karein:

```bash
loginctl show-user paradise -p Linger
systemctl --user status logserver.service --no-pager
podman ps -a
```

## 6. Lazmi Tasdeeq

Root:

```bash
loginctl show-user paradise -p Linger | grep 'Linger=yes'
```

Paradise ka direct session:

```bash
podman container exists logserver
systemctl --user status logserver.service --no-pager
podman inspect logserver --format '{{.State.Status}}'
```

Unit aur container ka reboot ke baad dobara chalna lazmi hai.

## 7. Students ko Jama Karne Wale Saboot

Podman version, Paradise identity, rootless status, image listing, Quadlet file, linger state, user-service status, container inspection, agar istemal huay hon to troubleshooting logs, aur reboot ka saboot jama karein.

## 8. Rollback ya Safai

Paradise user ke taur par:

```bash
systemctl --user stop logserver.service
rm -f ~/.config/containers/systemd/logserver.container
systemctl --user daemon-reload
podman rm -f logserver 2>/dev/null || true
```

Root ke taur par:

```bash
loginctl disable-linger paradise
```

## 9. Project Mukammal Karne ki Checklist

- [ ] Sahi VM ki tasdeeq ho gayi
- [ ] Zaroorat par snapshot bana liya gaya
- [ ] Asal halat record kar li gayi
- [ ] Configuration mukammal ho gayi
- [ ] Validation pass ho gayi
- [ ] SELinux ab bhi enforcing hai
- [ ] firewalld ab bhi enabled hai
- [ ] Zaroorat par reboot persistence test kar li gayi
- [ ] Saboot jama kar liye gaye
- [ ] Rollback samajh liya gaya

## 10. Jaiza Sawalat

1. Is project ne kaunsa karobari masla hal kiya?
2. Kis command ne sabit kiya ke configuration active thi?
3. Kis command ne sabit kiya ke configuration mustaqil thi?
4. Kya fail ho sakta hai, aur aap rollback kaise karenge?
