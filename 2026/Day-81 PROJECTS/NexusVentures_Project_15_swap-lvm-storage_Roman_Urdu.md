# NexusVentures Project 15: Swap aur LVM Database Storage

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

Instructor ki di hui khali disk par 512 MiB swap partition banayein aur VG `datastore` mein `database` naam ka LVM LV banayein. 8 MiB extents istemal karein, 50 extents allocate karein, ext3 format karein aur `/mnt/database` par mount karein.

## 2. Karobari Manzar Nama

NexusVentures emergency memory capacity aur database ke liye makhsoos filesystem add kar raha hai. Kaam mustaqil aur data ke liye mehfooz hona lazmi hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Chuni hui disk erase ho jayegi. Instructor ki ijazat aur Xen Orchestra snapshot lazmi hain.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Assigned khali disk ki pehchan karein

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL
blkid
LAB_DISK="/dev/sdb"
findmnt -S "$LAB_DISK"
wipefs -n "$LAB_DISK"
```

Kabhi yeh farz na karein ke disk `/dev/sdb` hai; instructor ki assigned device istemal karein.

### Qadam 2: Metadata save karein aur snapshot banayein

```bash
mkdir -p /root/nexusventures-project15/evidence
sfdisk -d "$LAB_DISK"   > /root/nexusventures-project15/evidence/partition-table-before.txt 2>/dev/null || true
cp -a /etc/fstab /root/nexusventures-project15/fstab-before
```

### Qadam 3: GPT partitions banayein

**Yeh amal chuni hui disk ko erase kar dega.**

```bash
parted -s "$LAB_DISK" mklabel gpt
parted -s "$LAB_DISK" mkpart swap linux-swap 1MiB 513MiB
parted -s "$LAB_DISK" set 1 swap on
parted -s "$LAB_DISK" mkpart lvm 513MiB 100%
parted -s "$LAB_DISK" set 2 lvm on
partprobe "$LAB_DISK"
udevadm settle
lsblk "$LAB_DISK"
```

`/dev/sdb` ki misaal mein:

```bash
SWAP_PART="/dev/sdb1"
LVM_PART="/dev/sdb2"
```

## Hissa A: Swap

### Qadam 4: Swap initialize karein aur mustaqil banayein

```bash
mkswap "$SWAP_PART"
SWAP_UUID=$(blkid -s UUID -o value "$SWAP_PART")
echo "UUID=$SWAP_UUID none swap defaults 0 0" >> /etc/fstab
swapon -a
swapon --show
free -h
```

## Hissa B: LVM

### Qadam 5: PV aur VG banayein

```bash
pvcreate "$LVM_PART"
vgcreate -s 8M datastore "$LVM_PART"
pvs
vgs -o vg_name,vg_size,vg_free,vg_extent_size
```

### Qadam 6: 50 extents banayein

```bash
lvcreate -l 50 -n database datastore
lvs -o lv_name,vg_name,lv_size,devices
```

50 × 8 MiB taqreeban 400 MiB hota hai.

### Qadam 7: ext3 banayein aur mount karein

```bash
dnf install -y e2fsprogs
mkfs.ext3 /dev/datastore/database
mkdir -p /mnt/database
DB_UUID=$(blkid -s UUID -o value /dev/datastore/database)
echo "UUID=$DB_UUID /mnt/database ext3 defaults 0 0" >> /etc/fstab
mount -a
```

### Qadam 8: Test karein

```bash
findmnt /mnt/database
df -hT /mnt/database
lsblk -f
swapon --show
echo 'NexusVentures database test' > /mnt/database/validation.txt
sync
```

### Qadam 9: Reboot karein aur tasdeeq karein

```bash
reboot
```

Phir:

```bash
swapon --show
findmnt /mnt/database
cat /mnt/database/validation.txt
vgs -o vg_name,vg_extent_size
lvs datastore
mount -a
```

## 6. Lazmi Tasdeeq

```bash
swapon --show | grep -F "$SWAP_PART"
vgs datastore -o vg_extent_size
lvs /dev/datastore/database
findmnt /mnt/database
test -f /mnt/database/validation.txt
mount -a
```

## 7. Students ko Jama Karne Wale Saboot

Instructor ki disk assignment, `lsblk` ka pehle aur baad ka output, partition table, swap UUID aur activation, fstab entries, PV/VG/LV output, 8 MiB extent size, 50 extents ka nateeja, filesystem aur mount output aur reboot ka saboot jama karein.

## 8. Rollback ya Safai

Ulti tarteeb mein unmount aur remove karein:

```bash
umount /mnt/database
swapoff "$SWAP_PART"
```

`/etc/fstab` se project ki dono lines hata dein, phir:

```bash
lvremove -y /dev/datastore/database
vgremove -y datastore
pvremove -y "$LVM_PART"
wipefs -a "$SWAP_PART" "$LVM_PART"
```

Mukammal rollback ke liye snapshot reversion istemal karein.

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
