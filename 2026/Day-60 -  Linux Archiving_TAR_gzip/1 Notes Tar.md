# MODULE 06/07 – TAR Command Notes
> **Archive Aur Files Transfer Karna (Roman Urdu)**

---

# 📖 Introduction

Is module mein hum seekhenge ke Linux mein files ko archive, compress aur transfer kaise kiya jata hai.

Ye har Linux System Administrator ke liye bohot important skill hai kyun ke rozana backups lena, files transfer karna aur data migrate karna common tasks hote hain.

---

# 🎯 Is Module Mein Kya Seekhenge?

Is module mein hum cover karenge:

- TAR archives ko manage karna
- TAR command ke common options
- Archive files create karna
- Archive ke contents dekhna
- Archive ko extract karna
- Compressed archives create karna
- Compressed archives extract karna
- Linux systems ke darmiyan securely files transfer karna
- Linux systems ke darmiyan files synchronize karna

---

# 1. Archive Kya Hota Hai?

**Archive** ek **single file** hoti hai jo multiple files aur directories ko apne andar rakhti hai.

Mukhtalif files ko alag alag manage karne ke bajaye un sab ko ek hi archive file mein combine kar diya jata hai.

Isay aap ek ZIP file ya ek badi file folder ki tarah samajh sakte hain.

---

# Archive Banane Ki Zaroorat Kyun Parti Hai?

Archives bohot useful hoti hain kyun ke ye:

- Backups ko aasaan banati hain.
- File transfer ko simple banati hain.
- Multiple files ko organize karti hain.
- Transfer hone wali files ki tadaad kam karti hain.
- Data ko manage karna aasaan bana deti hain.

---

# Example

Suppose aap ke paas hain:

- 10 Project Files
- 20 Configuration Files
- 15 Log Files

Total:

```text
45 Files
```

Un sab ko combine karke aap sirf ek file bana sakte hain.

---

# 2. TAR Command Kya Hai?

**tar** Linux ki standard utility hai jo use hoti hai:

- Archive create karne ke liye
- Archive ke contents dekhne ke liye
- Archive ko extract karne ke liye
- Archive files ko manage karne ke liye

TAR ka asal matlab hai:

> **Tape Archive**

Kyunkay pehle ye Magnetic Tape Drives par backup lene ke liye design ki gayi thi.

Aaj kal ye backups aur file transfer ke liye sab se zyada use hoti hai.

---

# 3. Archive File

TAR archive asal mein ek single regular file hoti hai jo apne andar rakhti hai:

- Files
- Directories
- Subdirectories

Archive ko aap store kar sakte hain:

- Hard Disk
- USB Flash Drive
- Tape Drive
- External Storage
- Network Storage

---

# Example

Agar aap ke paas:

```text
file1
file2
file3
file4
file5
```

hain to TAR in sab ko combine karke bana deta hai:

```text
backup.tar
```

Ab sirf ek file copy karni hogi.

---

# 4. TAR Kyun Use Karte Hain?

Suppose aap ke paas 10 bari files hain.

Requirement ye hai:

- Backup lena hai.
- Backup doosre Linux Server par bhejna hai.

Agar aap 10 alag alag files bhejein to zyada time lagega.

Behtar tareeqa ye hai:

### Step 1

Sab files ko ek archive mein combine karein.

```text
backup.tar
```

### Step 2

Ab sirf ek archive file network ke through transfer karein.

Is tarah backup aur transfer dono bohot aasaan ho jate hain.

---

# 5. TAR Ke Common Operations

TAR utility mukhtalif operations perform kar sakti hai.

| Operation | Kaam |
|------------|------|
| Create | Archive banana |
| List | Archive ke contents dekhna |
| Extract | Archive se files restore karna |

---

# 6. Archive Banana

TAR multiple files ko combine karke ek archive banata hai.

General syntax:

```bash
tar [options] archive_name files
```

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

# 7. Archive Ke Contents Dekhna

Archive ko extract kiye baghair us ke andar ki files dekh sakte hain.

Example:

```bash
tar -tvf backup.tar
```

Ye archive ke andar mojood tamam files display karega.

---

# 8. Archive Extract Karna

Archive se files restore karne ke liye:

```bash
tar -xvf backup.tar
```

Tamam files current directory mein extract ho jayengi.

---

# 9. Compression Kya Hoti Hai?

Compression ka matlab hai file ka size chhota karna.

Is ke faide:

- Network transfer tez hota hai.
- Backup chhota ho jata hai.
- Disk space bachti hai.
- Storage aasaan ho jati hai.

---

# Archive Compress Kyun Karte Hain?

Suppose aap ne archive banayi:

```text
backup.tar
```

Aur us ka size hai:

```text
10 GB
```

Agar isay compress kar diya jaye:

```text
backup.tar.gz
```

To size ho sakta hai:

```text
1 GB
```

Is se transfer bohot tez ho jata hai.

---

# 10. Compression Methods

Linux kai compression algorithms support karta hai.

---

## 1. gzip

Extension:

```text
.tar.gz
```

Characteristics:

- Sab se Fast
- Sab se Common
- Har platform par available

Best Use:

- Daily Backups
- File Transfers

---

## 2. bzip2

Extension:

```text
.tar.bz2
```

Characteristics:

- gzip se behtar compression
- Lekin thori slow

Best Use:

- Medium Size Backups

---

## 3. xz

Extension:

```text
.tar.xz
```

Characteristics:

- Sab se behtar compression ratio
- Sab se slow compression

Best Use:

- Large Archives
- Long-Term Storage

---

## 4. compress

Extension:

```text
.tar.Z
```

Characteristics:

- Bohot purana compression method
- Purane Unix systems mein use hota tha

Aaj kal bohot kam use hota hai.

---

# 11. Compression Comparison

| Compression | Extension | Speed | Compression |
|--------------|-----------|--------|-------------|
| gzip | `.tar.gz` | Sab se Fast | Good |
| bzip2 | `.tar.bz2` | Medium | Better |
| xz | `.tar.xz` | Slow | Best |
| compress | `.tar.Z` | Fast | Legacy |

---

# 12. Typical Backup Workflow

```text
Multiple Files
        │
        ▼
TAR Archive
        │
        ▼
Compression
        │
        ▼
Compressed Archive
        │
        ▼
Network Ke Through Transfer
        │
        ▼
Destination Server Par Extract
```

---

# 13. TAR Ke Advantages

TAR use karne ke bohot se faide hain.

- Multiple files ko ek archive mein combine karta hai.
- Backup ko aasaan banata hai.
- File transfer simplify karta hai.
- Compression utilities ke saath kaam karta hai.
- File permissions preserve karta hai.
- Directory structure preserve karta hai.

---

# 14. TAR Kab Use Karna Chahiye?

TAR use karein jab aap ko:

- Files ka backup lena ho.
- Directories ka backup lena ho.
- Projects transfer karne hon.
- Log files archive karni hon.
- Configuration files move karni hon.
- System backup banana ho.

---

# 15. Interview Questions

### Question 1

TAR ka full form kya hai?

**Answer**

Tape Archive

---

### Question 2

TAR command kis liye use hoti hai?

**Answer**

Archive create, list, manage aur extract karne ke liye.

---

### Question 3

Bohot sari files copy karne ke bajaye TAR kyun use karte hain?

**Answer**

Kyun ke TAR multiple files ko ek archive mein combine kar deta hai, jis se backup aur transfer dono aasaan ho jate hain.

---

### Question 4

TAR archive ko compress kyun karte hain?

**Answer**

Taake us ka size kam ho jaye aur network transfer tez ho jaye.

---

### Question 5

Sab se behtar compression kaunsi hai?

**Answer**

`xz`

---

### Question 6

Sab se fast compression kaunsi hai?

**Answer**

`gzip`

---

# 📌 Quick Revision

| Topic | Yaad Rakhein |
|---------|--------------|
| Archive Utility | `tar` |
| Archive Ka Purpose | Multiple files ko combine karna |
| Sab se Fast Compression | `gzip` |
| Sab se Best Compression | `xz` |
| Legacy Compression | `compress (.Z)` |
| gzip se Behtar | `bzip2` |
| Common Backup File | `backup.tar` |

---

# 💡 Yaad Rakhein

> **TAR Archive ko ek Suitcase ki tarah samjhein.**
>
> Jaise safar ke liye kapray aur saman ek suitcase mein rakhte hain, waise hi TAR multiple files aur folders ko ek archive file mein jama kar deta hai.
>
> Agar suitcase bohot bara ho jaye to usay compress kar dete hain taake kam jagah le aur aasani se doosri jagah transfer ho sake.
>
> Isi tarah **TAR + Compression** backups aur file transfer ko bohot tez, aasaan aur efficient bana dete hain.