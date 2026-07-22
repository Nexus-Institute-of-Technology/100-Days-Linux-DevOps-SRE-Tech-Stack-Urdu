# NexusVentures Project 20: Logserver Container ke Liye Mustaqil Journal Storage

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

Host ki mustaqil journal storage configure karein, journal files ko Paradise ki ownership wali directory mein rakhein aur us directory ko `logserver` ke andar `/var/log/journal` par mount karein.

## 2. Karobari Manzar Nama

NexusVentures chahta hai ke rootless logging container mustaqil journal saboot ka jaiza le, jabke host ka live journal mehfooz rahe.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Project 19 mukammal hona lazmi hai. User-service commands ke liye Paradise ka direct login istemal karein.

## 5. Qadam-ba-Qadam Hal

## Root Configuration

### Qadam 1: journald configuration ka backup lein

```bash
mkdir -p /root/nexusventures-project20/backups
cp -a /etc/systemd/journald.conf   /root/nexusventures-project20/backups/journald.conf-before
```

### Qadam 2: Drop-in ke zariye mustaqil storage configure karein

```bash
mkdir -p /etc/systemd/journald.conf.d
cat > /etc/systemd/journald.conf.d/10-nexus-persistent.conf <<'EOF'
[Journal]
Storage=persistent
EOF
systemd-analyze cat-config systemd/journald.conf
```

### Qadam 3: Mustaqil journal directory banayein aur label karein

```bash
mkdir -p /var/log/journal
systemd-tmpfiles --create --prefix /var/log/journal
restorecon -Rv /var/log/journal
systemctl restart systemd-journald
journalctl --flush
```

### Qadam 4: Host journal files ki tasdeeq karein

```bash
find /var/log/journal -type f -name '*.journal' -ls
journalctl --disk-usage
```

### Qadam 5: Copy ki gayi saboot directory tayyar karein

```bash
mkdir -p /home/paradise/container_journal
cp -a /var/log/journal/. /home/paradise/container_journal/
chown -R paradise:paradise /home/paradise/container_journal
restorecon -Rv /home/paradise/container_journal
find /home/paradise/container_journal -maxdepth 3   -printf '%M %u:%g %p\n' | head -30
```

Yeh project live host journal ko read-write expose karne ki bajaye saboot ki copy banata hai.

## Paradise ka Direct Session

### Qadam 6: Quadlet ka backup lein

```bash
cp -a ~/.config/containers/systemd/logserver.container   ~/.config/containers/systemd/logserver.container.before-project20
```

### Qadam 7: Bind mount add karein

Edit karein:

```bash
vi ~/.config/containers/systemd/logserver.container
```

`[Container]` section ke neeche add karein:

```text
Volume=/home/paradise/container_journal:/var/log/journal:ro,Z
```

`ro` write operations ko rokta hai. `Z` private SELinux container label apply karta hai.

### Qadam 8: Reload aur restart karein

```bash
systemctl --user daemon-reload
systemctl --user restart logserver.service
systemctl --user status logserver.service --no-pager
```

### Qadam 9: Mount metadata ka jaiza lein

```bash
podman inspect logserver   --format '{{range .Mounts}}{{.Source}} -> {{.Destination}} ({{.Options}}){{println}}{{end}}'
```

### Qadam 10: Container ke andar visibility ki tasdeeq karein

```bash
podman exec logserver   sh -c 'find /var/log/journal -type f -name "*.journal" -ls | head'
```

Agar image mein shell na ho to mount-inspection ka saboot jama karein aur instructor se approved diagnostic image istemal karein.

### Qadam 11: SELinux aur services ki tasdeeq karein

```bash
getenforce
systemctl is-active systemd-journald
ausearch -m AVC -ts recent | tail -20
```

Koi unresolved AVC access ko nahin rokna chahiye.

### Qadam 12: Reboot ke baad tasdeeq

```bash
reboot
```

Reboot ke baad Paradise ke taur par:

```bash
systemctl --user status logserver.service --no-pager
podman ps -a
podman inspect logserver   --format '{{range .Mounts}}{{.Source}} -> {{.Destination}}{{println}}{{end}}'
podman exec logserver   sh -c 'find /var/log/journal -type f -name "*.journal" | head'
```

## 6. Lazmi Tasdeeq

Root:

```bash
systemctl is-active systemd-journald
find /var/log/journal -type f -name '*.journal' | grep -q .
test -d /home/paradise/container_journal
getenforce | grep Enforcing
```

Paradise:

```bash
systemctl --user status logserver.service --no-pager
podman inspect logserver | grep -F '/var/log/journal'
podman exec logserver test -d /var/log/journal
```

## 7. Students ko Jama Karne Wale Saboot

journald drop-in aur effective configuration, host ki mustaqil journal listing, disk usage, copied directory ki ownership, Quadlet volume line, container mount inspection, container ke andar ki listing, SELinux status, service status aur reboot ka saboot jama karein.

## 8. Rollback ya Safai

Paradise user ke taur par:

```bash
cp -a ~/.config/containers/systemd/logserver.container.before-project20   ~/.config/containers/systemd/logserver.container
systemctl --user daemon-reload
systemctl --user restart logserver.service
```

Root ke taur par:

```bash
rm -f /etc/systemd/journald.conf.d/10-nexus-persistent.conf
systemctl restart systemd-journald
rm -rf /home/paradise/container_journal
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
