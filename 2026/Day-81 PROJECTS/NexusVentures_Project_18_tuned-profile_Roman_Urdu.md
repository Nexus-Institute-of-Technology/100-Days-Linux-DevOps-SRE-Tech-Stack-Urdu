# NexusVentures Project 18: TuneD ke Sath Khudkar Performance Profile ka Intikhab

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

TuneD install karein aur system ke liye recommend kiya gaya profile activate karein.

## 2. Karobari Manzar Nama

NexusVentures chahta hai ke operating system har VM ke liye munasib performance aur power-management policy apply kare.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.


## 5. Qadam-ba-Qadam Hal

### Qadam 1: System type record karein

```bash
lscpu
systemd-detect-virt
```

### Qadam 2: TuneD install aur enable karein

```bash
dnf install -y tuned
systemctl enable --now tuned
systemctl status tuned --no-pager
```

### Qadam 3: Maujooda profile record karein

```bash
mkdir -p /root/nexusventures-project18/evidence
tuned-adm active > /root/nexusventures-project18/evidence/profile-before.txt
```

### Qadam 4: Recommendation query karein

```bash
RECOMMENDED_PROFILE=$(tuned-adm recommend)
echo "$RECOMMENDED_PROFILE"
tuned-adm list
```

Virtual machines ko aksar `virtual-guest` recommend hota hai, lekin asal recommendation hi istemal karein.

### Qadam 5: Apply karein aur tasdeeq karein

```bash
tuned-adm profile "$RECOMMENDED_PROFILE"
tuned-adm active
tuned-adm verify
systemctl is-active tuned
systemctl is-enabled tuned
```

### Qadam 6: Saboot save karein

```bash
tuned-adm recommend > /root/nexusventures-project18/evidence/recommended.txt
tuned-adm active > /root/nexusventures-project18/evidence/profile-after.txt
tuned-adm verify > /root/nexusventures-project18/evidence/verify.txt
```

### Qadam 7: Reboot karein aur dobara test karein

```bash
reboot
```

Phir:

```bash
systemctl is-active tuned
tuned-adm active
tuned-adm verify
```

## 6. Lazmi Tasdeeq

```bash
systemctl is-enabled tuned
systemctl is-active tuned
tuned-adm active
tuned-adm verify
```

Active profile ko recorded recommendation ke mutabiq hona lazmi hai, jab tak instructor ne kisi doosre profile ki ijazat na di ho.

## 7. Students ko Jama Karne Wale Saboot

Virtualization detection, recommendation, dastiyab profiles, active profile ka pehle/baad ka output, TuneD verification, service state aur reboot ka saboot jama karein.

## 8. Rollback ya Safai

`profile-before.txt` mein record kiya gaya pichla profile apply karein, ya TuneD disable karein:

```bash
tuned-adm off
systemctl disable --now tuned
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
