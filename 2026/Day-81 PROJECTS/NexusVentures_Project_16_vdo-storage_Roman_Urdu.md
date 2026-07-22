# NexusVentures Project 16: Thin-Provisioned VDO Storage Volume

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`vectra` naam ka VDO-backed volume banayein jis ka logical size 50 GiB ho aur use `/test` par mount karein.

## 2. Karobari Manzar Nama

NexusVentures dohraye jane wale application data ke liye storage deduplication, compression aur thin provisioning ka jaiza le raha hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Yeh amal chuni hui disk ko erase karega. Khali device aur physical allocation instructor se approved honi lazmi hai.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Tools install karein

```bash
dnf install -y lvm2 vdo kmod-kvdo xfsprogs
```

Yeh project Rocky Linux 9 par LVM-integrated VDO istemal karta hai. Purane material mein standalone `vdo create` command nazar aa sakti hai.

### Qadam 2: Ghair istemal shuda disk ki pehchan karein

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL
VDO_DISK="/dev/sdc"
findmnt -S "$VDO_DISK"
wipefs -n "$VDO_DISK"
```

Sirf instructor ki assigned device istemal karein.

### Qadam 3: Snapshot banayein aur khali disk clear karein

```bash
wipefs -a "$VDO_DISK"
```

### Qadam 4: PV aur VG banayein

```bash
pvcreate "$VDO_DISK"
vgcreate vdovg "$VDO_DISK"
pvs
vgs
```

### Qadam 5: Physical aur logical sizes chunain

```bash
PHYSICAL_SIZE="5G"
VIRTUAL_SIZE="50G"
vgs vdovg -o vg_size,vg_free
```

Physical size disk ke andar fit honi chahiye. Virtual size woh logical capacity hai jo filesystem ko dikhayi jati hai.

### Qadam 6: VDO LV banayein

```bash
lvcreate --type vdo   --name vectra   --size "$PHYSICAL_SIZE"   --virtualsize "$VIRTUAL_SIZE"   vdovg
```

### Qadam 7: Topology ka jaiza lein

```bash
lvs -a -o lv_name,vg_name,lv_attr,lv_size,pool_lv,data_percent,metadata_percent
```

### Qadam 8: Format aur mount karein

```bash
mkfs.xfs /dev/vdovg/vectra
mkdir -p /test
VDO_UUID=$(blkid -s UUID -o value /dev/vdovg/vectra)
cp -a /etc/fstab /root/fstab-before-project16
echo "UUID=$VDO_UUID /test xfs defaults 0 0" >> /etc/fstab
mount -a
```

### Qadam 9: Logical size ki tasdeeq karein

```bash
findmnt /test
df -hT /test
lvs -a -o lv_name,lv_size,data_percent,metadata_percent vdovg
```

### Qadam 10: Compressible sample data likhein

```bash
dd if=/dev/zero of=/test/zero-data.bin bs=1M count=500 status=progress
sync
du -h /test/zero-data.bin
df -h /test
lvs -a -o lv_name,lv_size,data_percent,metadata_percent vdovg
vdostats --human-readable 2>/dev/null || true
```

### Qadam 11: Reboot karein aur tasdeeq karein

```bash
reboot
```

Phir:

```bash
findmnt /test
df -hT /test
lvs -a vdovg
test -f /test/zero-data.bin
mount -a
```

## 6. Lazmi Tasdeeq

```bash
lvs vdovg/vectra
findmnt /test
test "$(findmnt -n -o FSTYPE /test)" = "xfs"
df -h /test
mount -a
```

Physical allocation choti hone ke bawajood logical filesystem taqreeban 50 GiB ke qareeb hona chahiye.

## 7. Students ko Jama Karne Wale Saboot

Assigned disk, package list, PV/VG/LV output, physical aur virtual sizes, VDO topology, XFS aur fstab ka saboot, mount output, statistics, sample-data test aur reboot ka saboot jama karein.

## 8. Rollback ya Safai

```bash
umount /test
```

Project wali fstab line hata dein, phir:

```bash
lvremove -y /dev/vdovg/vectra
vgremove -y vdovg
pvremove -y "$VDO_DISK"
wipefs -a "$VDO_DISK"
rmdir /test
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
