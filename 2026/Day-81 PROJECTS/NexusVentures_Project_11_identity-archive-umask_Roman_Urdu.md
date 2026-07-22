# NexusVentures Project 11: Shanakht, Archive aur Private Default Permissions

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

Teen munsalik tasks mukammal karein: UID 1326 ke sath user `alies` banayein; `/var/tmp` ko `/root/test.tar.gz` ke taur par archive karein; aur Natasha ki nayi files ka default mode 400 aur directories ka mode 500 configure karein.

## 2. Karobari Manzar Nama

NexusVentures ko ek muqarrar application identity, compressed saboot ka backup aur sensitive user ke liye bohat private default workspace chahiye.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Project 04 mein Natasha ban chuki honi chahiye.

## 5. Qadam-ba-Qadam Hal

## Hissa A: Muqarrar UID Wala Account Banayein

### Qadam 1: Conflicts check karein

```bash
getent passwd alies
getent passwd 1326
```

Agar UID 1326 kisi doosre account ki ho to aage na barhein.

### Qadam 2: Account banayein

```bash
useradd -m -u 1326 alies
passwd alies
```

Instructor ka assigned lab password istemal karein. Exam example mein `alies` istemal hua hai; lab se bahar kamzor passwords istemal na karein.

### Qadam 3: Tasdeeq karein

```bash
id alies
getent passwd alies
```

## Hissa B: `/var/tmp` ka Archive Banayein

### Qadam 4: gzip archive banayein

```bash
tar -czvf /root/test.tar.gz -C / var/tmp
```

`-C /` istemal karne se absolute path ki bajaye relative path `var/tmp` store hota hai.

### Qadam 5: Archive ki tasdeeq karein

```bash
gzip -t /root/test.tar.gz
tar -tzf /root/test.tar.gz | head -30
ls -lh /root/test.tar.gz
```

### Qadam 6: Ikhtiyari restore test

```bash
mkdir -p /root/project11-restore
tar -xzf /root/test.tar.gz -C /root/project11-restore
find /root/project11-restore -maxdepth 3 | head -30
```

## Hissa C: Natasha ka umask Configure Karein

### Qadam 7: Profile ka backup lein

```bash
cp -a /home/natasha/.bash_profile   /home/natasha/.bash_profile.before-project11
```

### Qadam 8: umask 0277 add karein

```bash
grep -qxF 'umask 0277' /home/natasha/.bash_profile ||   echo 'umask 0277' >> /home/natasha/.bash_profile
chown natasha:natasha /home/natasha/.bash_profile
```

### Qadam 9: Naye login shell ki tasdeeq karein

```bash
runuser -l natasha -c 'umask'
```

Mutawaqqa nateeja: `0277`.

### Qadam 10: Test objects banayein

```bash
runuser -l natasha -c '
rm -f ~/project11-file
rm -rf ~/project11-directory
touch ~/project11-file
mkdir ~/project11-directory
stat -c "%A %a %U:%G %n" ~/project11-file ~/project11-directory
'
```

Mutawaqqa nateeja:

```text
file: 400
directory: 500
```

Files ka maximum shuruati mode 666 aur directories ka 777 hota hai. Mask 0277 lagane se owner ki write permission, tamam group permissions aur tamam other permissions hat jati hain.

## 6. Lazmi Tasdeeq

```bash
test "$(id -u alies)" = "1326"
gzip -t /root/test.tar.gz
tar -tzf /root/test.tar.gz | grep '^var/tmp'
test "$(runuser -l natasha -c 'umask')" = "0277"
test "$(stat -c '%a' /home/natasha/project11-file)" = "400"
test "$(stat -c '%a' /home/natasha/project11-directory)" = "500"
```

## 7. Students ko Jama Karne Wale Saboot

UID availability check, `id alies`, archive size aur listing, gzip integrity test, Natasha profile ki tabdeeli, umask output, file aur directory modes aur duplicate UIDs ke khatre ki wazahat jama karein.

## 8. Rollback ya Safai

```bash
userdel -r alies
rm -f /root/test.tar.gz
rm -rf /root/project11-restore
cp -a /home/natasha/.bash_profile.before-project11 /home/natasha/.bash_profile
rm -f /home/natasha/project11-file
rm -rf /home/natasha/project11-directory
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
