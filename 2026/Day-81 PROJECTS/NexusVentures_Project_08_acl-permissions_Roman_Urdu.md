# NexusVentures Project 08: ACLs ke Sath Bareek Satah ka File Access

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`/etc/fstab` ko `/var/tmp/fstab` par copy karein; root ownership barqarar rakhein; execute access hata dein; Harry ko read/write dein; Natasha ko mana karein; aur tamam doosre users ko read access dein.

## 2. Karobari Manzar Nama

NexusVentures ko user-specific exceptions chahiye jo mamooli owner/group/other mode bits se zaher nahin ki ja sakti.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Project 04 mein Harry, Natasha aur Sarah ban chuke hone chahiye.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: ACL tools install karein aur copy banayein

```bash
dnf install -y acl
cp -p /etc/fstab /var/tmp/fstab
chown root:root /var/tmp/fstab
chmod 0644 /var/tmp/fstab
setfacl -b /var/tmp/fstab
```

### Qadam 2: Named-user entries add karein

```bash
setfacl -m u:harry:rw- /var/tmp/fstab
setfacl -m u:natasha:--- /var/tmp/fstab
setfacl -m m::rw- /var/tmp/fstab
```

### Qadam 3: Jaiza lein

```bash
ls -l /var/tmp/fstab
getfacl /var/tmp/fstab
```

`ls -l` mein `+` extended ACL information ko zahir karta hai.

### Qadam 4: Harry ko test karein

```bash
runuser -u harry -- head -n 1 /var/tmp/fstab
runuser -u harry -- sh -c 'echo "# Harry ACL test" >> /var/tmp/fstab'
```

### Qadam 5: Natasha ko test karein

```bash
runuser -u natasha -- cat /var/tmp/fstab
runuser -u natasha -- sh -c 'echo fail >> /var/tmp/fstab'
```

Dono operations ka fail hona lazmi hai.

### Qadam 6: Kisi doosre user ko test karein

```bash
runuser -u sarah -- head -n 1 /var/tmp/fstab
```

Sarah ko `other::r--` ke zariye read access milta hai.

### Qadam 7: Tasdeeq karein ke execute bit nahin hai

```bash
find /var/tmp/fstab -perm /111 -print
```

Koi output aana mutawaqqa nahin hai.

### Qadam 8: Saboot save karein

```bash
mkdir -p /root/nexusventures-project08/evidence
getfacl /var/tmp/fstab > /root/nexusventures-project08/evidence/fstab-acl.txt
stat /var/tmp/fstab > /root/nexusventures-project08/evidence/fstab-stat.txt
```

## 6. Lazmi Tasdeeq

```bash
test "$(stat -c '%U:%G' /var/tmp/fstab)" = "root:root"
! find /var/tmp/fstab -perm /111 | grep -q .
getfacl /var/tmp/fstab | grep 'user:harry:rw-'
getfacl /var/tmp/fstab | grep 'user:natasha:---'
getfacl /var/tmp/fstab | grep 'other::r--'
runuser -u harry -- test -w /var/tmp/fstab
! runuser -u natasha -- test -r /var/tmp/fstab
runuser -u sarah -- test -r /var/tmp/fstab
```

## 7. Students ko Jama Karne Wale Saboot

`ls -l`, `getfacl`, owner/group, Harry ke read/write ka saboot, Natasha ka denial, Sarah ke read access ka saboot aur ACL mask ki wazahat jama karein.

## 8. Rollback ya Safai

```bash
rm -f /var/tmp/fstab
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
