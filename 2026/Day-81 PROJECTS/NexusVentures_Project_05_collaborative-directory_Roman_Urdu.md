# NexusVentures Project 05: Mehfooz Mushtarka Administration Workspace

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`/common/admin` banayein jiska group owner `admin` ho, group members ko full access ho, doosron ko koi access na ho, aur nayi files khudkar `admin` group inherit karein.

## 2. Karobari Manzar Nama

NexusVentures administrators ko ek control shuda mushtarka workspace chahiye jahan nayi files team ke group ke sath munsalik rahen.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- `whoami` se account ki tasdeeq karein; mutawaqqa output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
- Change se pehle ka saboot `/root/nexusventures-project05/` ke andar save karein.
Project 04 ka mukammal hona zaroori hai.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Dependencies ki tasdeeq karein

```bash
getent group admin
id harry
id natasha
id sarah
```

### Qadam 2: Banayein aur configure karein

```bash
mkdir -p /common/admin
chown root:admin /common/admin
chmod 2770 /common/admin
```

Shuru ka `2` setgid bit set karta hai. Nayi files `admin` group inherit karti hain.

### Qadam 3: Jaiza lein

```bash
ls -ld /common/admin
stat -c '%A %a %U:%G %n' /common/admin
```

### Qadam 4: Majaz users ko test karein

```bash
runuser -u harry -- touch /common/admin/harry-file
runuser -u natasha -- touch /common/admin/natasha-file
stat -c '%U:%G %n' /common/admin/harry-file /common/admin/natasha-file
```

Mutawaqqa group: `admin`.

### Qadam 5: Sarah ka denial test karein

```bash
runuser -u sarah -- ls /common/admin
runuser -u sarah -- touch /common/admin/should-fail
```

Dono operations fail hone chahiye.

### Qadam 6: Reboot karein aur dobara karein

```bash
reboot
```

Phir:

```bash
stat -c '%a %U:%G' /common/admin
runuser -u harry -- touch /common/admin/post-reboot-file
stat -c '%G' /common/admin/post-reboot-file
```

## 6. Lazmi Tasdeeq

```bash
test "$(stat -c '%a' /common/admin)" = "2770"
test "$(stat -c '%G' /common/admin)" = "admin"
runuser -u harry -- test -w /common/admin
runuser -u natasha -- test -w /common/admin
! runuser -u sarah -- test -r /common/admin
test "$(stat -c '%G' /common/admin/harry-file)" = "admin"
```

## 7. Students ko Jama Karne Wale Saboot

Directory mode aur ownership, dono admin members ki kamyab file creation, Sarah ke liye denial, inherited group ownership aur reboot ke baad ka saboot jama karein.

## 8. Rollback ya Safai

```bash
rm -rf /common/admin
rmdir /common 2>/dev/null || true
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
