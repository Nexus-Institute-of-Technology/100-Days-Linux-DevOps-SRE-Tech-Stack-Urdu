# NexusVentures Project 10: Bari Configuration Files ki Talaash aur Jama Karna

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`/etc` ke andar 4 MiB se bari tamam regular files talash karein aur unhein `/find/largefiles` mein copy karein.

## 2. Karobari Manzar Nama

NexusVentures `/etc` ke andar ghair mutawaqqa disk growth ki tehqiq kar raha hai. Students ko matching files ko ek jaisay basenames ke takrao ke baghair mehfooz karna hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.


## 5. Qadam-ba-Qadam Hal

### Qadam 1: Tayyari aur inventory banayein

```bash
mkdir -p /find/largefiles
find /etc -xdev -type f -size +4M -printf '%s %p\n' | sort -n   | tee /root/project10-source-inventory.txt
```

### Qadam 2: Jab match na mile to controlled test file banayein

Sirf instructor ki ijazat ke sath:

```bash
dd if=/dev/zero of=/etc/nexusventures-large-test.bin bs=1M count=5 status=progress
```

Inventory dobara chalayein.

### Qadam 3: Paths ko mehfooz rakhte hue copy karein

```bash
cd /
find etc -xdev -type f -size +4M   -exec cp --parents --preserve=mode,timestamps -t /find/largefiles {} +
```

Parent paths ko mehfooz rakhne se ek hi basename wali do files ek doosre ko overwrite nahin karti.

### Qadam 4: Destination ka jaiza lein

```bash
find /find/largefiles -type f -printf '%s %p\n' | sort -n
du -sh /find/largefiles
```

### Qadam 5: Counts ka muqabla karein

```bash
SOURCE_COUNT=$(find /etc -xdev -type f -size +4M | wc -l)
DEST_COUNT=$(find /find/largefiles -type f | wc -l)
printf 'Source=%s\nDestination=%s\n' "$SOURCE_COUNT" "$DEST_COUNT"
test "$SOURCE_COUNT" -eq "$DEST_COUNT"
```

### Qadam 6: Checksums ka muqabla karein

```bash
cd /
while IFS= read -r source; do
  relative="${source#/}"
  destination="/find/largefiles/$relative"
  sha256sum "$source" "$destination"
done < <(find /etc -xdev -type f -size +4M)
```

### Qadam 7: Destination inventory save karein

```bash
find /find/largefiles -type f -exec sha256sum {} +   > /root/project10-destination-sha256.txt
```

## 6. Lazmi Tasdeeq

```bash
test -d /find/largefiles
SOURCE_COUNT=$(find /etc -xdev -type f -size +4M | wc -l)
DEST_COUNT=$(find /find/largefiles -type f | wc -l)
test "$SOURCE_COUNT" -eq "$DEST_COUNT"
find /find/largefiles -type f -size +4M | grep -q .
```

## 7. Students ko Jama Karne Wale Saboot

Source aur destination inventories, counts, disk usage, checksum comparisons aur `-xdev`, `+4M` aur `--parents` ki wazahat jama karein.

## 8. Rollback ya Safai

```bash
rm -rf /find/largefiles
rmdir /find 2>/dev/null || true
rm -f /etc/nexusventures-large-test.bin
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
