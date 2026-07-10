# MODULE 07 – Practice Lab: TAR Archive Extract Karna
> **Hands-on Practice Lab – TAR Archive Extract Karna Aur Directories Ka Backup Banana (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- TAR archive ke contents ko dekhna.
- TAR archive se files extract karna.
- Archive se files restore karna.
- Poori directory ka backup banana.
- Archive ka size verify karna.
- Samajhna ke TAR archive backups aur file transfer ko kis tarah aasaan banata hai.

---

# 📖 Introduction

Pichlay practice lab mein hum ne seekha tha ke TAR archive kaise create ki jati hai.

Ab sochiye ke aap ne ye archive ek Linux server se doosre Linux server par transfer kar di hai.

Ab agla step ye hai ke us archive ke andar mojood files ko restore (extract) kiya jaye.

Linux archive ko extract karne ke liye **tar** command use karta hai.

---

# Pichlay Lab Ka Review

Pichlay lab mein hum ne ek archive create ki thi:

```text
boot.log.tar
```

Ye archive multiple boot log files ka backup hai.

Multiple files transfer karne ke bajaye hum sirf ek archive file transfer karte hain.

Jab archive destination server par pohanch jaye to usay aasani se extract kiya ja sakta hai.

---

# 🔬 Lab 1 – Archive Ke Contents Dekhna

Archive ko extract karne se pehle ye dekhna ek achhi practice hai ke us ke andar kaunsi files mojood hain.

Command:

```bash
tar -tf boot.log.tar
```

### Explanation

| Option | Matlab |
|---------|---------|
| `-t` | Archive ke contents list karo |
| `-f` | Archive ka filename specify karo |

Example Output:

```text
boot.log
boot.log.1
boot.log.2
boot.log.old
```

Is se confirm ho jata hai ke archive ke andar kaunsi files hain.

---

# 🔬 Lab 2 – TAR Archive Extract Karna

Archive se tamam files extract karne ke liye:

```bash
tar -xvf boot.log.tar
```

### Explanation

| Option | Matlab |
|---------|---------|
| `-x` | Files extract karo |
| `-v` | Verbose output dikhao |
| `-f` | Archive ka filename specify karo |

Command execute hone ke baad har extract hone wali file screen par nazar aayegi.

Extraction ke baad tamam files current directory mein aa jayengi.

---

# 🔬 Lab 3 – Extracted Files Verify Karein

Directory ke contents dekhein.

```bash
ls
```

Aap ko ab ye files nazar aani chahiye:

```text
boot.log
boot.log.1
boot.log.2
boot.log.old
```

Ye confirm karta hai ke archive successfully extract ho chuka hai.

---

# Practical Scenario

Suppose aap ke paas do Linux Servers hain.

- Linux Server A
- Linux Server B

### Step 1

Server A par archive create karein.

```text
boot.log.tar
```

### Step 2

Archive ko Server B par transfer karein.

### Step 3

Server B par archive extract karein.

```bash
tar -xvf boot.log.tar
```

Ab archive ke andar mojood tamam original files restore ho jayengi.

---

# 🔬 Lab 4 – Poori Directory Ka Backup Banana

TAR sirf files hi nahi balkay poori directories ka bhi archive bana sakta hai.

Linux ki sab se important directories mein se ek hai:

```text
/etc
```

Is directory ke andar mojood hoti hain:

- System configuration files
- Service configuration
- Network configuration
- User configuration
- Security configuration

`/etc` ka backup lena Linux Administrator ka bohot common task hai.

---

# 🔬 Lab 5 – `/etc` Directory Ka Archive Banana

Command:

```bash
tar -cvf /root/etcbackup.tar /etc
```

### Explanation

| Part | Matlab |
|------|---------|
| `tar` | Archive utility |
| `-c` | Archive create karo |
| `-v` | Verbose output show karo |
| `-f` | Archive ka filename |
| `/root/etcbackup.tar` | Archive ka naam aur location |
| `/etc` | Jis directory ka backup lena hai |

Ye command create karegi:

```text
/root/etcbackup.tar
```

Jismein poori `/etc` directory ka backup hoga.

---

# 🔬 Lab 6 – Archive Verify Karein

Archive ko verify karne ke liye:

```bash
ls -lh /root/etcbackup.tar
```

Example Output:

```text
-rw-r--r-- root root 26M etcbackup.tar
```

`-h` ka matlab hai:

Human Readable Format

Yani size KB, MB ya GB mein show hogi.

---

# 🔬 Lab 7 – Archive Ka Size Check Karein

Archive ka size dekhne ka ek aur tareeqa:

```bash
du -sh /root/etcbackup.tar
```

Example Output:

```text
26M
```

Is ka matlab archive taqreeban **26 MB** ka hai.

---

# 🧪 Practice Exercises

---

## Exercise 1

Archive ke contents dekhein.

```bash
tar -tf boot.log.tar
```

---

## Exercise 2

Archive extract karein.

```bash
tar -xvf boot.log.tar
```

---

## Exercise 3

Extracted files verify karein.

```bash
ls
```

---

## Exercise 4

`/etc` directory ka backup banayein.

```bash
tar -cvf /root/etcbackup.tar /etc
```

---

## Exercise 5

Archive verify karein.

```bash
ls -lh /root/etcbackup.tar
```

---

## Exercise 6

Archive ka size check karein.

```bash
du -sh /root/etcbackup.tar
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Aap bhool gaye ke archive ke andar kaunsi files hain.

Command:

```bash
tar -tf archive.tar
```

---

### Scenario 2

Aap ko doosre server se archive mili hai aur usay restore karna hai.

Command:

```bash
tar -xvf archive.tar
```

---

### Scenario 3

Configuration changes karne se pehle `/etc` directory ka backup lena hai.

Command:

```bash
tar -cvf /root/etcbackup.tar /etc
```

---

### Scenario 4

Backup archive ka size verify karna hai.

Command:

```bash
du -sh /root/etcbackup.tar
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `tar -tf archive.tar` | Archive ke contents dekho |
| `tar -xvf archive.tar` | Archive extract karo |
| `tar -cvf /root/etcbackup.tar /etc` | `/etc` ka backup banao |
| `ls -lh archive.tar` | Archive ka size dekho |
| `du -sh archive.tar` | Archive ki disk usage dekho |

---

# 📖 Key Takeaways

- `tar -tf` archive ko extract kiye baghair us ke contents dikhata hai.
- `tar -xvf` archive ke andar ki tamam files ko extract karta hai.
- TAR archives Linux servers ke darmiyan files transfer karne ke liye bohot useful hain.
- Poori directories, jaise `/etc`, ka bhi archive banaya ja sakta hai.
- Archive create karne ke baad hamesha usay verify karein.
- Archive ka size dekhne ke liye `ls -lh` ya `du -sh` use karein.

---

# 💡 Yaad Rakhein

> **TAR Archive ko ek band Storage Box ki tarah samjhein.**
>
> Agar aap dekhna chahein ke box ke andar kya rakha hai to pehle box kholne ki zarurat nahi.
>
> Sirf ye command chalayein:
>
> ```bash
> tar -tf archive.tar
> ```
>
> Aur jab aap tamam files wapas hasil karna chahein to:
>
> ```bash
> tar -xvf archive.tar
> ```
>
> TAR Linux System Administrators ke liye backup, migration aur file transfer ka sab se important tools mein se ek hai.