# NexusVentures Project 17: Online LVM aur Filesystem ko Barhana

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`/mnt/database` par mounted `database` logical volume ko **mazeed 100 extents** se barhayein.

## 2. Karobari Manzar Nama

NexusVentures database storage ko maujooda data khoye baghair control shuda online barhotri chahiye.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Project 15 mukammal hona chahiye aur VG mein kam az kam 100 khali extents hone chahiye.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Project 15 ke resources ki tasdeeq karein

```bash
findmnt /mnt/database
lvs /dev/datastore/database
vgs datastore
```

### Qadam 2: Baseline record karein

```bash
mkdir -p /root/nexusventures-project17/evidence
lvs -o lv_name,vg_name,lv_size,lv_attr   > /root/nexusventures-project17/evidence/lvs-before.txt
vgs -o vg_name,vg_extent_size,vg_free_count,vg_free   > /root/nexusventures-project17/evidence/vgs-before.txt
df -hT /mnt/database   > /root/nexusventures-project17/evidence/df-before.txt
sha256sum /mnt/database/validation.txt   > /root/nexusventures-project17/evidence/validation-before.sha256
```

### Qadam 3: Khali extents check karein

```bash
vgs datastore -o vg_free_count,vg_extent_size
```

Kam az kam 100 khali extents zaroori hain. 8 MiB extents ke sath barhotri taqreeban 800 MiB hogi.

### Qadam 4: 100 extents se barhayein

```bash
lvextend -l +100 -r /dev/datastore/database
```

Plus sign bohat zaroori hai. `-l 100` total 100 extents mangta hai; `-l +100` mazeed 100 add karta hai. `-r` LV barhane ke baad filesystem ko bhi barhata hai.

### Qadam 5: Size aur mount ki tasdeeq karein

```bash
lvs -o lv_name,vg_name,lv_size /dev/datastore/database
df -hT /mnt/database
findmnt /mnt/database
```

### Qadam 6: Data integrity sabit karein

```bash
sha256sum -c /root/nexusventures-project17/evidence/validation-before.sha256
cat /mnt/database/validation.txt
```

### Qadam 7: Baad ki halat save karein

```bash
lvs -o lv_name,vg_name,lv_size,lv_attr   > /root/nexusventures-project17/evidence/lvs-after.txt
vgs -o vg_name,vg_extent_size,vg_free_count,vg_free   > /root/nexusventures-project17/evidence/vgs-after.txt
df -hT /mnt/database   > /root/nexusventures-project17/evidence/df-after.txt
```

### Qadam 8: Reboot karein aur tasdeeq karein

```bash
reboot
```

Phir:

```bash
findmnt /mnt/database
df -hT /mnt/database
cat /mnt/database/validation.txt
lvs /dev/datastore/database
```

## 6. Lazmi Tasdeeq

LV ko 50 extents se barh kar 150 extents hona chahiye:

```bash
lvs /dev/datastore/database
findmnt /mnt/database
sha256sum -c /root/nexusventures-project17/evidence/validation-before.sha256
mount -a
```

## 7. Students ko Jama Karne Wale Saboot

Pehle ke free extents, bilkul sahi `lvextend` command, LV aur filesystem size ka pehle/baad ka output, checksum ka saboot, mount evidence aur reboot ke baad validation jama karein.

## 8. Rollback ya Safai

Volume ko chota karna barhane se zyada khatarnak hai. Classroom rollback ke liye pre-change snapshot istemal karein. Approved offline filesystem-shrink plan ke baghair `lvreduce` na chalayein.

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
