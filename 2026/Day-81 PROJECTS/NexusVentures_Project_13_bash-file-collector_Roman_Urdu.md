# NexusVentures Project 13: Mazboot Bash File Collection Automation

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`mysearch` naam ka executable script banayein jo `/usr/share` ke andar 1 MiB se choti regular files talash kare aur unhein `/root/myfiles` ke andar copy kare.

## 2. Karobari Manzar Nama

NexusVentures ko dobara chalne wala evidence collector chahiye jo filenames ke spaces aur duplicate basenames ko mehfooz tareeqe se handle kare.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.


## 5. Qadam-ba-Qadam Hal

### Qadam 1: Capacity check karein

```bash
df -h /root
```

### Qadam 2: Script banayein

```bash
cat > /root/mysearch <<'EOF'
#!/bin/bash
set -euo pipefail

SOURCE="/usr/share"
DESTINATION="/root/myfiles"

if [[ ! -d "$SOURCE" ]]; then
    echo "ERROR: missing source: $SOURCE" >&2
    exit 1
fi

rm -rf "$DESTINATION"
mkdir -p "$DESTINATION"
count=0

while IFS= read -r -d '' file
do
    relative="${file#/}"
    target="$DESTINATION/$relative"
    mkdir -p "$(dirname "$target")"
    cp --preserve=mode,timestamps "$file" "$target"
    count=$((count + 1))
done < <(find "$SOURCE" -xdev -type f -size -1M -print0)

echo "Copied $count files from $SOURCE to $DESTINATION"
EOF
```

### Qadam 3: Permissions set karein aur syntax validate karein

```bash
chown root:root /root/mysearch
chmod 0750 /root/mysearch
bash -n /root/mysearch
```

### Qadam 4: Mutawaqqa files ginen

```bash
EXPECTED=$(find /usr/share -xdev -type f -size -1M | wc -l)
echo "$EXPECTED"
```

### Qadam 5: Chalayein

```bash
/root/mysearch | tee /root/mysearch-run.txt
```

### Qadam 6: Counts ka muqabla karein

```bash
ACTUAL=$(find /root/myfiles -type f | wc -l)
printf 'Expected=%s\nActual=%s\n' "$EXPECTED" "$ACTUAL"
test "$EXPECTED" -eq "$ACTUAL"
```

### Qadam 7: Jaiza lein aur rule ki tasdeeq karein

```bash
find /root/myfiles -type f | head -20
du -sh /root/myfiles
find /root/myfiles -type f ! -size -1M -print
```

Aakhri command ko koi output nahin dena chahiye.

### Qadam 8: Doosri martaba chalayein

```bash
/root/mysearch
```

Script ko duplicate barhotri ke baghair destination dobara banana chahiye.

## 6. Lazmi Tasdeeq

```bash
bash -n /root/mysearch
test -x /root/mysearch
EXPECTED=$(find /usr/share -xdev -type f -size -1M | wc -l)
ACTUAL=$(find /root/myfiles -type f | wc -l)
test "$EXPECTED" -eq "$ACTUAL"
! find /root/myfiles -type f ! -size -1M | grep -q .
```

## 7. Students ko Jama Karne Wale Saboot

Script source, mode, syntax validation, mutawaqqa aur asal counts, run output, sample destination tree, disk usage aur `set -euo pipefail` aur `-print0` ki wazahat jama karein.

## 8. Rollback ya Safai

```bash
rm -f /root/mysearch /root/mysearch-run.txt
rm -rf /root/myfiles
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
