# Linux Practice Lab – tar aur Compression ko Samajhna
# DAY-59
> JULY 05, 2026
## Maqsad (Objective)

Is lab ke baad aap seekh jayenge ke:

- tar archive banana
- Mukhtalif compression algorithms istemal karna
- Archive sizes compare karna
- Har compression method ke faide aur nuqsanat samajhna
- Archive ke andar ki files dekhna
- Archive ko extract (unpack) karna

---

# Lab Environment

- Operating System: Rocky Linux 9
- User: root (ya sudo user)
- Working Directory:

```text
/mnt/backup
```

---

# Scenario

Aap ke Linux server mein `/etc` directory ke andar tamam important configuration files mojood hain.

Aik Linux System Administrator ki haisiyat se aap ka kaam hai ke in files ka backup mukhtalif compression methods ke sath banayen aur dekhen ke kaunsa method sab se zyada disk space bachata hai.

---

# Task 1 – Backup Directory Banana

Backup directory banayen.

```bash
mkdir -p /mnt/backup
```

Ab backup directory ke andar jayen.

```bash
cd /mnt/backup
```

Apni current location verify karein.

```bash
pwd
```

Expected Output

```text
/mnt/backup
```

---

# Task 2 – Simple tar Archive Banana

Bila compression ke aik normal tar archive banayen.

```bash
tar -cvf /mnt/backup/etcbackup.tar /etc
```

### Explanation

| Option | Matlab |
|---------|---------|
| -c | Naya archive create karo |
| -v | Har file ka naam screen par dikhayo |
| -f | Archive file ka naam specify karo |

Aap notice karenge:

```text
tar: Removing leading '/' from member names
```

Ye bilkul normal behavior hai.

Linux jaan boojh kar leading `/` hata deta hai taake jab archive extract ho to woh system ke root filesystem ko overwrite na kar de.

---

# Task 3 – gzip Compression ke Sath Archive Banana

```bash
tar -cvzf /mnt/backup/etcbackup.tar.gz /etc
```

### Explanation

| Option | Matlab |
|---------|---------|
| -z | gzip compression use karo |

---

# Task 4 – bzip2 Compression ke Sath Archive Banana

```bash
tar -cvjf /mnt/backup/etcbackup.tar.bz2 /etc
```

### Explanation

| Option | Matlab |
|---------|---------|
| -j | bzip2 compression use karo |

---

# Task 5 – xz Compression ke Sath Archive Banana

```bash
tar -cvJf /mnt/backup/etcbackup.tar.xz /etc
```

### Explanation

| Option | Matlab |
|---------|---------|
| -J | xz compression use karo |

---

# Task 6 – Backup Files Dekhna

Tamam backup files dekhen.

```bash
ls -lh
```

Example

```text
-rw-r--r-- etcbackup.tar
-rw-r--r-- etcbackup.tar.gz
-rw-r--r-- etcbackup.tar.bz2
-rw-r--r-- etcbackup.tar.xz
```

### Sawal

Sab se bari file kaunsi hai?

Jawab

```text
etcbackup.tar
```

Kyun?

Is liye ke is mein koi compression nahi hui.

---

# Task 7 – Disk Space Compare Karna

Command chalayein.

```bash
du -sk *
```

Example Output

```text
22000   etcbackup.tar
4744    etcbackup.tar.gz
3872    etcbackup.tar.bz2
1080    etcbackup.tar.xz
```

---

# Observation

| Archive | Taqreeban Size |
|-----------|---------------:|
| etcbackup.tar | 22 MB |
| etcbackup.tar.gz | 4.7 MB |
| etcbackup.tar.bz2 | 3.8 MB |
| etcbackup.tar.xz | 1.1 MB |

---

# Discussion

### Sab se chhoti file kis compression ne banayi?

Jawab

```text
xz
```

### Sab se tez archive kis ne banaya?

Jawab

```text
tar (No Compression)
```

### Linux mein sab se zyada commonly konsa compression use hota hai?

Jawab

```text
gzip (.tar.gz)
```

---

# Compression Algorithms ko Samajhna

| Compression | Extension | Speed | Compression |
|-------------|-----------|-------|-------------|
| None | .tar | Sab se Fast | Bilkul nahi |
| gzip | .tar.gz | Fast | Achhi |
| bzip2 | .tar.bz2 | Thori Slow | gzip se Behtar |
| xz | .tar.xz | Sab se Slow | Sab se Behtar |

---

# Task 8 – Archive ki Files Dekhna (Extract Kiye Baghair)

Simple tar archive.

```bash
tar -tvf etcbackup.tar
```

gzip archive.

```bash
tar -tvzf etcbackup.tar.gz
```

bzip2 archive.

```bash
tar -tvjf etcbackup.tar.bz2
```

xz archive.

```bash
tar -tvJf etcbackup.tar.xz
```

### Sawal

Kya is command se files extract hui?

Jawab

```text
Nahi
```

Sirf archive ke andar mojood files ki list nazar aati hai.

---

# Task 9 – Archives Extract Karna

Directories banayen.

```bash
mkdir extract-tar
mkdir extract-gzip
mkdir extract-bzip2
mkdir extract-xz
```

Simple tar extract karein.

```bash
tar -xvf etcbackup.tar -C extract-tar
```

gzip extract karein.

```bash
tar -xvzf etcbackup.tar.gz -C extract-gzip
```

bzip2 extract karein.

```bash
tar -xvjf etcbackup.tar.bz2 -C extract-bzip2
```

xz extract karein.

```bash
tar -xvJf etcbackup.tar.xz -C extract-xz
```

Verify karein.

```bash
ls extract-tar
```

---

# Task 10 – Extracted Files Verify Karna

Original directory dekhen.

```bash
ls /etc
```

Extracted directory dekhen.

```bash
ls extract-tar/etc
```

### Sawal

Kya dono directories mein same files hain?

Jawab

```text
Ji Haan
```

---

# Common tar Options

| Option | Matlab |
|---------|---------|
| -c | Archive create karo |
| -x | Archive extract karo |
| -t | Archive ke andar ki files dekho |
| -v | Verbose mode |
| -f | Archive filename |
| -z | gzip compression |
| -j | bzip2 compression |
| -J | xz compression |
| -C | Extraction directory change karo |

---

# RHCSA Challenge 1

Notes dekhe baghair:

`/var/log` ka gzip backup banayen.

File ka naam rakhen.

```text
logs.tar.gz
```

Aur is location mein save karein.

```text
/mnt/backup
```

---

# RHCSA Challenge 2

Archive ko extract kiye baghair us ke andar ki tamam files dekhen.

---

# RHCSA Challenge 3

Archive ko extract karein.

Location:

```text
/tmp/logrestore
```

---

# RHCSA Challenge 4

Kaunsi command har archive ki disk space usage dikhati hai?

---

# RHCSA Challenge 5

Kaunsi command human-readable file size dikhati hai?

---

# RHCSA Challenge 6

Aam tor par sab se behtar compression kaunsi hoti hai?

---

# Knowledge Check

1. `-c` kis liye use hota hai?

2. `-x` kis liye use hota hai?

3. `-t` kis liye use hota hai?

4. `-v` kis liye use hota hai?

5. `-f` kis liye use hota hai?

6. gzip compression ke liye konsa option use hota hai?

7. bzip2 compression ke liye konsa option use hota hai?

8. xz compression ke liye konsa option use hota hai?

9. Is lab mein sab se chhoti file kis compression ne banayi?

10. tar ye message kyun dikhata hai?

```text
Removing leading '/' from member names
```

---

# Summary

Is lab ke baad aap confidently:

✅ tar archives bana sakte hain.

✅ gzip compression use kar sakte hain.

✅ bzip2 compression use kar sakte hain.

✅ xz compression use kar sakte hain.

✅ Archive sizes compare kar sakte hain.

✅ Archive ke andar ki files dekh sakte hain.

✅ Archives extract kar sakte hain.

✅ Har compression algorithm ke faide aur use cases samajh sakte hain.

Ye tamam skills RHCSA exam aur real production Linux servers dono mein bohot important hain.