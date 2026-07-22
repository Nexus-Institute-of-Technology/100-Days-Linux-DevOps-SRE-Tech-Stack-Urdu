# NexusVentures Project 12: Password Aging aur Tafweez Shuda Administration

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

Naye users ke liye default maximum password age 20 din set karein aur `admin` group ke members ko password ke baghair sudo istemal karne ki ijazat dein.

## 2. Karobari Manzar Nama

NexusVentures account lifecycle policy lagu kar raha hai aur administration approved Linux support group ko tafweez kar raha hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Project 04 mein group `admin`, Harry, Natasha aur Sarah maujood hone chahiye.

## 5. Qadam-ba-Qadam Hal

## Hissa A: Password Aging

### Qadam 1: Login defaults ka backup lein

```bash
mkdir -p /root/nexusventures-project12/backups
cp -a /etc/login.defs /root/nexusventures-project12/backups/login.defs-before
```

### Qadam 2: Default ko edit karein

```bash
vi /etc/login.defs
```

Set karein:

```text
PASS_MAX_DAYS   20
```

Tasdeeq karein:

```bash
grep -nE '^[[:space:]]*PASS_MAX_DAYS[[:space:]]+' /etc/login.defs
```

### Qadam 3: Test user banayein

```bash
useradd -m nexusnew
passwd nexusnew
chage -l nexusnew
```

Maximum days ki tadaad 20 honi chahiye. Maujooda users khudkar modify nahin hote.

## Hissa B: Admin Group ke Liye Password ke Baghair sudo

### Qadam 4: sudo install karein

```bash
dnf install -y sudo
```

### Qadam 5: Drop-in banayein

```bash
cat > /etc/sudoers.d/nexus-admin <<'EOF'
%admin ALL=(ALL) NOPASSWD: ALL
EOF
chown root:root /etc/sudoers.d/nexus-admin
chmod 0440 /etc/sudoers.d/nexus-admin
```

### Qadam 6: Test se pehle syntax ki tasdeeq karein

```bash
visudo -cf /etc/sudoers
visudo -cf /etc/sudoers.d/nexus-admin
```

### Qadam 7: Effective privileges ka jaiza lein

```bash
sudo -l -U harry
sudo -l -U natasha
```

### Qadam 8: Harry ko test karein

```bash
runuser -l harry -c 'sudo -n /usr/bin/id'
```

Mutawaqqa output mein `uid=0(root)` hona chahiye.

### Qadam 9: Sabit karein ke Sarah ke paas privilege nahin

```bash
runuser -l sarah -c 'sudo -n /usr/bin/id'
```

Is ka fail hona lazmi hai.

### Qadam 10: Reboot karein aur dobara test karein

```bash
reboot
```

Reboot ke baad:

```bash
chage -l nexusnew
visudo -cf /etc/sudoers
runuser -l harry -c 'sudo -n /usr/bin/id'
```

## 6. Lazmi Tasdeeq

```bash
grep -Eq '^PASS_MAX_DAYS[[:space:]]+20$' /etc/login.defs
chage -l nexusnew | grep -i 'Maximum number of days' | grep 20
visudo -cf /etc/sudoers
test "$(stat -c '%a' /etc/sudoers.d/nexus-admin)" = "440"
runuser -l harry -c 'sudo -n /usr/bin/id' | grep 'uid=0'
! runuser -l sarah -c 'sudo -n /usr/bin/id'
```

## 7. Students ko Jama Karne Wale Saboot

login.defs ka pehle aur baad ka saboot, `chage -l nexusnew`, sudoers drop-in aur permissions, `visudo` output, Harry ki kamyabi, Sarah ka denial aur reboot test jama karein. `NOPASSWD: ALL` ke risk ki wazahat karein.

## 8. Rollback ya Safai

```bash
cp -a /root/nexusventures-project12/backups/login.defs-before /etc/login.defs
rm -f /etc/sudoers.d/nexus-admin
userdel -r nexusnew
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
