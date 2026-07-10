# MODULE 07 – Practice Lab: TAR Archive Banana
> **Hands-on Practice Lab – TAR Archive Create Karna Aur Us Ke Contents Dekhna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- TAR archive kaise create karte hain.
- TAR command ke common options ko samajhna.
- Multiple files ko ek single archive mein convert karna.
- Archive successfully create hua ya nahi us ko verify karna.
- Archive ko doosri location par move karna.
- Archive ko extract kiye baghair us ke contents dekhna.

---

# 📖 Introduction

Linux System Administrator ki sab se common responsibilities mein se ek **backup banana** hota hai.

Har file ko alag alag copy karne ke bajaye Linux humein **tar** utility provide karta hai jo multiple files ko ek single archive mein combine kar deta hai.

Is se:

- Backup lena aasaan ho jata hai.
- Files transfer karna aasaan ho jata hai.
- Storage manage karna aasaan ho jata hai.

---

# TAR Command Ko Samjhein

Basic command hai:

```bash
tar
```

`tar` command ke saath hamesha options use kiye jate hain jo batate hain ke humein kya operation perform karna hai.

---

# Common TAR Options

| Option | Matlab |
|---------|---------|
| `-c` | Naya archive create karo |
| `-v` | Verbose mode (progress show karo) |
| `-f` | Archive ka filename specify karo |
| `-t` | Archive ke contents list karo |
| `-x` | Archive se files extract karo |

---

# Archive Banane Ki Syntax

General syntax:

```bash
tar -cvf archive_name.tar file1 file2 file3
```

### Explanation

- `-c` → Naya archive create karo.
- `-v` → Har file ka naam display karo jab archive ban raha ho.
- `-f` → Archive ka filename specify karo.

Example:

```bash
tar -cvf backup.tar file1 file2 file3
```

Ye command:

```text
backup.tar
```

create karegi jismein teeno files hongi.

---

# 🔬 Lab 1 – Files Ki Location Par Jayein

Sab se pehle us directory mein jayein jahan multiple files mojood hon.

Example:

```bash
cd /var/log
```

Files ki list dekhein.

```bash
ls
```

Yahan aap ko multiple log files nazar aayengi.

---

# 🔬 Lab 2 – TAR Archive Create Karein

Is lab mein hum **boot logs** ka archive banayenge.

Command:

```bash
tar -cvf boot.log.tar boot.log*
```

### Explanation

| Part | Matlab |
|------|---------|
| `tar` | Archive utility |
| `-c` | Archive create karo |
| `-v` | Verbose output show karo |
| `-f` | Archive filename specify karo |
| `boot.log.tar` | Archive ka naam |
| `boot.log*` | Jitni bhi files `boot.log` se shuru hoti hain un sab ko include karo |

Wildcard (`*`) tamam matching files ko include karta hai.

Example:

```text
boot.log
boot.log.1
boot.log.2
boot.log.old
```

Ye tamam files archive ke andar chali jayengi.

---

# 🔬 Lab 3 – Archive Verify Karein

Directory ke contents dekhein.

```bash
ls -l
```

Ab aap ko ye file nazar aani chahiye:

```text
boot.log.tar
```

Ye confirm karta hai ke archive successfully create ho gaya hai.

---

# 🔬 Lab 4 – Backup Directory Banayein

`/tmp` ke andar ek nayi directory create karein.

```bash
mkdir /tmp/backup
```

Verify karein.

```bash
ls /tmp
```

---

# 🔬 Lab 5 – Archive Ko Move Karein

Archive ko backup directory mein move karein.

```bash
mv boot.log.tar /tmp/backup/
```

Verify karein.

```bash
ls /tmp/backup
```

Expected Output:

```text
boot.log.tar
```

---

# 🔬 Lab 6 – Archive Ke Contents Dekhein

TAR ki ek bohot useful feature ye hai ke archive ko extract kiye baghair us ke andar ki files dekh sakte hain.

Command:

```bash
tar -tf /tmp/backup/boot.log.tar
```

### Explanation

| Option | Matlab |
|---------|---------|
| `-t` | Archive ke contents list karo |
| `-f` | Archive filename specify karo |

Example Output:

```text
boot.log
boot.log.1
boot.log.2
boot.log.old
```

Is se confirm ho jata hai ke archive ke andar kaunsi files mojood hain.

---

# Wildcard (`*`) Ka Istemaal

Har filename alag alag likhne ke bajaye:

```bash
tar -cvf backup.tar file1 file2 file3 file4
```

Aap wildcard use kar sakte hain.

Example:

```bash
tar -cvf backup.tar file*
```

Ye command un tamam files ko archive karegi jo:

```text
file
```

se shuru hoti hain.

Example:

```text
file1
file2
file3
fileA
file_backup
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Log directory mein jayein.

```bash
cd /var/log
```

---

## Exercise 2

Tamam files ki list dekhein.

```bash
ls
```

---

## Exercise 3

TAR archive create karein.

```bash
tar -cvf boot.log.tar boot.log*
```

---

## Exercise 4

Verify karein ke archive create ho gaya hai.

```bash
ls -l
```

---

## Exercise 5

Backup directory create karein.

```bash
mkdir /tmp/backup
```

---

## Exercise 6

Archive ko move karein.

```bash
mv boot.log.tar /tmp/backup/
```

---

## Exercise 7

Backup directory verify karein.

```bash
ls /tmp/backup
```

---

## Exercise 8

Archive ke contents dekhein.

```bash
tar -tf /tmp/backup/boot.log.tar
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Archive create ho gaya hai lekin aap ko yaqeen nahi ke us mein kaunsi files hain.

Command:

```bash
tar -tf boot.log.tar
```

---

### Scenario 2

Ghalat files archive ho gayi hain.

Solution:

Purana archive delete karein.

```bash
rm boot.log.tar
```

Phir dobara archive create karein.

```bash
tar -cvf boot.log.tar boot.log*
```

---

### Scenario 3

Aap bohot sari files ko archive karna chahte hain bina har filename likhe.

Solution:

Wildcard use karein.

```bash
tar -cvf backup.tar *
```

Ya

```bash
tar -cvf backup.tar file*
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `tar -cvf backup.tar files` | TAR archive create kare |
| `tar -tf backup.tar` | Archive ke contents dekhein |
| `mkdir /tmp/backup` | Backup directory create kare |
| `mv backup.tar /tmp/backup` | Archive move kare |
| `ls -l` | Archive verify kare |

---

# 📖 Key Takeaways

- `tar -c` naya archive create karta hai.
- `-v` archive banate waqt progress show karta hai.
- `-f` archive ka filename specify karta hai.
- Wildcard (`*`) ki madad se multiple files ko aasani se archive kiya ja sakta hai.
- `tar -tf` archive ko extract kiye baghair us ke contents dikhata hai.
- TAR archives backup aur file transfer ko bohot aasaan bana dete hain.

---

# 💡 Yaad Rakhein

> **TAR Archive ko ek Storage Box ki tarah samjhein.**
>
> Jaise ghar ka saman ek box mein rakh dete hain, waise hi TAR multiple files ko ek archive file mein jama kar deta hai.
>
> Agar aap dekhna chahein ke box ke andar kya rakha hai, to pehle box kholne ki zarurat nahi.
>
> Sirf ye command chalayein:
>
> ```bash
> tar -tf archive.tar
> ```
>
> Ye aap ko archive ke andar mojood tamam files ki list dikha dega, bina unhein extract kiye.