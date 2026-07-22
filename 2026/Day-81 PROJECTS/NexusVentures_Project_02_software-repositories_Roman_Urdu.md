# NexusVentures Project 02: Control Shuda Software Repository Configuration

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

BaseOS aur AppStream repositories configure karein. Sample URLs `http://repo.eight.example.com/BaseOS` aur `http://repo.eight.example.com/AppStream` hain.

## 2. Karobari Manzar Nama

NexusVentures sirf approved repositories se software install karta hai. Students repository metadata define karenge, package ki dastiyabi verify karenge aur agle project ke liye Apache install karenge.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- `whoami` se account ki tasdeeq karein; mutawaqqa output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
- Change se pehle ka saboot `/root/nexusventures-project02/` ke andar save karein.


## 5. Qadam-ba-Qadam Hal

### Qadam 1: Reachable URLs hasil karein

```bash
BASEOS_URL="http://repo.eight.example.com/BaseOS"
APPSTREAM_URL="http://repo.eight.example.com/AppStream"
```

Instructor ki taraf se di gayi URLs istemal karein. Sample names sirf us waqt kaam karenge jab lab mein un ke mutabiq DNS aur web content maujood ho.

### Qadam 2: Maujooda repositories record karein

```bash
mkdir -p /root/nexusventures-project02/evidence
dnf repolist all > /root/nexusventures-project02/evidence/repolist-before.txt
```

### Qadam 3: Locations test karein

```bash
curl -I --max-time 10 "$BASEOS_URL/"
curl -I --max-time 10 "$APPSTREAM_URL/"
```

### Qadam 4: Backup lein aur repository file banayein

```bash
[ ! -f /etc/yum.repos.d/nexusventures.repo ] ||   cp -a /etc/yum.repos.d/nexusventures.repo   /root/nexusventures-project02/nexusventures.repo.before

cat > /etc/yum.repos.d/nexusventures.repo <<EOF
[nexus-baseos]
name=NexusVentures BaseOS
baseurl=${BASEOS_URL}
enabled=1
gpgcheck=0

[nexus-appstream]
name=NexusVentures AppStream
baseurl=${APPSTREAM_URL}
enabled=1
gpgcheck=0
EOF
```

`gpgcheck=0` alag exam-style lab ke liye hai. Production repositories mein bharosemand signatures aur keys istemal honi chahiye.

### Qadam 5: Sirf in repositories ko refresh karein

```bash
dnf clean all
dnf makecache --disablerepo='*'   --enablerepo=nexus-baseos,nexus-appstream

dnf repolist --disablerepo='*'   --enablerepo=nexus-baseos,nexus-appstream
```

### Qadam 6: Tasdeeq karein aur Apache install karein

```bash
dnf info httpd --disablerepo='*'   --enablerepo=nexus-baseos,nexus-appstream

dnf install -y httpd --disablerepo='*'   --enablerepo=nexus-baseos,nexus-appstream
rpm -q httpd
```

Project 03 shuru hone tak Apache start na karein.

## 6. Lazmi Tasdeeq

```bash
dnf makecache --disablerepo='*' --enablerepo=nexus-baseos,nexus-appstream
dnf repolist --disablerepo='*' --enablerepo=nexus-baseos,nexus-appstream
rpm -q httpd
```

## 7. Students ko Jama Karne Wale Saboot

`.repo` file, URL tests, `dnf repolist`, metadata refresh aur Apache package version jama karein. Repository IDs, `enabled`, `baseurl` aur `gpgcheck` ki wazahat karein.

## 8. Rollback ya Safai

```bash
rm -f /etc/yum.repos.d/nexusventures.repo
dnf clean all
```
Agar backup file maujood thi to use bahal karein.

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
