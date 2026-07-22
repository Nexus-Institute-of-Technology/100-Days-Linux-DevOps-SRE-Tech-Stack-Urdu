# NexusVentures Project 04: User Shanakht aur Administrative Group ki Tayyari

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`admin` group banayein; `harry` aur `natasha` ko supplementary members ke taur par add karein; `sarah` ko `/sbin/nologin` ke sath banayein aur use `admin` ka member na banayein; training passwords assign karein.

## 2. Karobari Manzar Nama

NexusVentures do junior administrators aur ek noninteractive service identity ko onboard kar raha hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- `whoami` se account ki tasdeeq karein; mutawaqqa output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
- Change se pehle ka saboot `/root/nexusventures-project04/` ke andar save karein.


## 5. Qadam-ba-Qadam Hal

### Qadam 1: Group banayein

```bash
groupadd admin
```

Agar lab dobara kar rahe hain to pehle `getent group admin` chalayein.

### Qadam 2: Harry aur Natasha ko banayein

```bash
useradd -m -G admin harry
passwd harry
useradd -m -G admin natasha
passwd natasha
```

Instructor ka assigned lab password istemal karein. Asal environment mein `password` lafz ko password ke taur par kabhi istemal na karein.

### Qadam 3: Sarah ko banayein

```bash
useradd -m -s /sbin/nologin sarah
passwd sarah
```

### Qadam 4: Account data ki tasdeeq karein

```bash
id harry
id natasha
id sarah
getent group admin
getent passwd harry natasha sarah
pwck -r
grpck -r
```

### Qadam 5: Sarah ki pabandi test karein

```bash
runuser -l sarah -c 'id'
```

Account unavailable ka message aana mutawaqqa hai.

### Qadam 6: Saboot save karein

```bash
mkdir -p /root/nexusventures-project04/evidence
id harry > /root/nexusventures-project04/evidence/harry.txt
id natasha > /root/nexusventures-project04/evidence/natasha.txt
id sarah > /root/nexusventures-project04/evidence/sarah.txt
getent group admin > /root/nexusventures-project04/evidence/admin-group.txt
```

## 6. Lazmi Tasdeeq

```bash
getent group admin
id harry | grep admin
id natasha | grep admin
! id sarah | grep -q admin
getent passwd sarah | grep '/sbin/nologin$'
pwck -r
grpck -r
```

## 7. Students ko Jama Karne Wale Saboot

Tamam users ka `id` output, admin group record, passwd records, Sarah ka failed interactive-shell test aur primary aur supplementary groups ke farq ki wazahat jama karein.

## 8. Rollback ya Safai

Sirf dependent projects mukammal hone ke baad:

```bash
userdel -r harry
userdel -r natasha
userdel -r sarah
groupdel admin
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
