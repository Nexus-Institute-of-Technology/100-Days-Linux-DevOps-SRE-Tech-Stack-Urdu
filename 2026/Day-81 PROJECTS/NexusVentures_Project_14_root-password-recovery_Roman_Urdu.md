# NexusVentures Project 14: GRUB ke Zariye Root Password Recovery

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

GRUB ko interrupt karke, `rd.break` istemal karke, chroot mein dakhil ho kar aur SELinux relabeling request karke bhoola hua root password reset karein.

## 2. Karobari Manzar Nama

NexusVentures training server ke root credentials kho gaye hain. Students ko OS dobara install kiye baghair majaz access bahal karna hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Sirf assigned training VM istemal karein. Baghair ijazat password recovery mana hai.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Snapshot banayein

Makhsoos recovery VM istemal karein aur `PRE-ROOT-RECOVERY-PROJECT14` snapshot banayein.

### Qadam 2: Xen Orchestra console istemal karein

Reboot karein. GRUB menu par:

1. Normal Rocky Linux entry ko highlight karein.
2. `e` dabayein.
3. `linux`, `linuxefi` ya isi tarah shuru hone wali line talash karein.
4. Aakhir mein `rd.break` add karein.
5. `Ctrl+x` dabayein.

### Qadam 3: Asal root filesystem ko dobara mount karein

Emergency prompt par:

```bash
mount -o remount,rw /sysroot
mount | grep ' /sysroot '
```

### Qadam 4: Installed system mein dakhil hon

```bash
chroot /sysroot
```

### Qadam 5: Root password reset karein

```bash
passwd root
```

Instructor se approved temporary password istemal karein. Jama kiye jane wale saboot mein password shamil na karein.

### Qadam 6: SELinux relabeling request karein

```bash
touch /.autorelabel
ls -l /.autorelabel
```

### Qadam 7: Do martaba exit karein

```bash
exit
exit
```

Relabeling mein kai minute lag sakte hain.

### Qadam 8: Console login ki tasdeeq karein

Root ke taur par login karein, phir:

```bash
whoami
id
getenforce
ls -lZ /etc/shadow
systemctl --failed
```

### Qadam 9: Normal reboot karein

```bash
reboot
```

GRUB ko edit kiye baghair dobara normal root login ki tasdeeq karein.

## 6. Lazmi Tasdeeq

Students ko kamyab root login, `uid=0(root)`, SELinux enforcing, `/etc/shadow` par durust context, koi ghair wazeh failed service na hone aur doosre normal boot ka saboot dena hoga.

## 7. Students ko Jama Karne Wale Saboot

Snapshot ka naam, likhi hui GRUB sequence, `/sysroot` remount, chroot, `/.autorelabel` ka saboot, recovery ke baad root identity, SELinux state, shadow file context aur normal reboot ka saboot jama karein. Password kabhi jama na karein.

## 8. Rollback ya Safai

`passwd root` se doosra approved password set karein, ya agar VM ko nuksan hua ho to project se pehle wale snapshot par revert karein.

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
