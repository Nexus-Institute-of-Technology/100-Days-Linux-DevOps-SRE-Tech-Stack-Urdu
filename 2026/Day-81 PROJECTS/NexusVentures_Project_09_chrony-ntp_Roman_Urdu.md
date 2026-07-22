# NexusVentures Project 09: Chrony ke Sath Bharosemand Waqt ki Ham-Ahangi

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

VM ko assigned server ka NTP client configure karein. Exam example mein `system2.eight.example.com` ka address `192.168.55.151` hai.

## 2. Karobari Manzar Nama

NexusVentures ko authentication, logs, incident ki tehqiq aur transactions ki durust tarteeb ke liye sahi waqt chahiye.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.


## 5. Qadam-ba-Qadam Hal

### Qadam 1: Approved source hasil karein

```bash
NTP_SERVER="192.168.55.151"
```

Server ko UDP port 123 par NTP provide karna lazmi hai.

### Qadam 2: Install karein aur backup lein

```bash
dnf install -y chrony
mkdir -p /root/nexusventures-project09/{backups,evidence}
cp -a /etc/chrony.conf /root/nexusventures-project09/backups/chrony.conf-before
chronyc tracking > /root/nexusventures-project09/evidence/tracking-before.txt 2>&1 || true
chronyc sources -v > /root/nexusventures-project09/evidence/sources-before.txt 2>&1 || true
```

### Qadam 3: Server add karein

```bash
vi /etc/chrony.conf
```

Add karein:

```text
server 192.168.55.151 iburst
```

Instructor ki di hui value istemal karein.

### Qadam 4: Tasdeeq karein aur syntax validate karein

```bash
grep -nE '^[[:space:]]*(server|pool)[[:space:]]' /etc/chrony.conf
chronyd -p -f /etc/chrony.conf
```

### Qadam 5: Enable aur restart karein

```bash
systemctl enable chronyd
systemctl restart chronyd
systemctl status chronyd --no-pager
```

### Qadam 6: Measurements hasil karein

```bash
chronyc online
chronyc burst 4/4
sleep 10
chronyc tracking
chronyc sources -v
chronyc sourcestats -v
timedatectl
```

`^*` selected source hai, `^+` usable source hai aur `^?` ka matlab hai ke abhi valid measurement nahin mila.

### Qadam 7: Logs ka jaiza lein

```bash
journalctl -u chronyd -n 30 --no-pager
```

### Qadam 8: Reboot karein aur tasdeeq karein

```bash
reboot
```

Phir:

```bash
systemctl is-active chronyd
systemctl is-enabled chronyd
chronyc tracking
chronyc sources -v
timedatectl
```

## 6. Lazmi Tasdeeq

```bash
chronyd -p -f /etc/chrony.conf >/dev/null
systemctl is-active chronyd
systemctl is-enabled chronyd
grep -E "^[[:space:]]*server[[:space:]]+$NTP_SERVER([[:space:]]|$)" /etc/chrony.conf
chronyc tracking
chronyc sources -v
```

## 7. Students ko Jama Karne Wale Saboot

Configuration backup, source directive, syntax validation, service status, tracking aur sources output, timedatectl, journal output aur reboot ka saboot jama karein.

## 8. Rollback ya Safai

```bash
cp -a /root/nexusventures-project09/backups/chrony.conf-before /etc/chrony.conf
chronyd -p -f /etc/chrony.conf
systemctl restart chronyd
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
