# MODULE 07 – Practice Lab: Archived Files Ki Listing
> **Hands-on Practice Lab – TAR Aur Compressed Archives Ke Contents Dekhna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- TAR archive ke contents ko dekhna.
- Compressed archives ke contents list karna.
- Samajhna ke `tar` command automatically compression format ko detect karta hai.
- Archive ko extract karne se pehle us ke contents verify karna.
- Mukhtalif compressed archives ke liye ek hi command use karna.

---

# 📖 Introduction

Pichlay practice labs mein hum ne mukhtalif compression methods use karke archives create ki thin.

Un mein shamil hain:

- Normal TAR Archive
- gzip Compressed Archive
- bzip2 Compressed Archive
- xz Compressed Archive

Archive ko extract karne se pehle ye dekhna bohot achhi practice hai ke us ke andar kaunsi files mojood hain.

Linux humein ye kaam **tar** command ki madad se karne ki sahulat deta hai.

---

# Archive Ke Contents Kyun Dekhte Hain?

Archive ke contents list karne ke bohot se faide hain.

Ye aap ko madad deta hai:

- Backup verify karne mein.
- Confirm karne mein ke kaunsi files archive ke andar hain.
- Ghair zaroori files ko extract karne se bachne mein.
- Backup restore karne se pehle usay check karne mein.

Ye khas tor par bohot bari backup files ke liye bohot useful hai.

---

# TAR Command Se Archive Ke Contents Dekhna

Basic command hai:

```bash
tar -tf archive_name
```

### Explanation

| Option | Matlab |
|---------|---------|
| `-t` | Archive ke contents list karo |
| `-f` | Archive ka filename specify karo |

---

# 🔬 Lab 1 – Normal TAR Archive Ke Contents Dekhna

Suppose aap ke paas ek normal TAR archive hai:

```text
etcbackup.tar
```

Command:

```bash
tar -tf /root/etcbackup.tar
```

Example Output:

```text
etc/
etc/passwd
etc/group
etc/fstab
etc/hosts
```

Ye archive ke andar mojood tamam files ko dikhata hai bina unhein extract kiye.

---

# 🔬 Lab 2 – gzip Compressed Archive Ke Contents Dekhna

Suppose archive **gzip** se compress ki gayi hai.

Archive:

```text
etcbackup.tar.gz
```

Command:

```bash
tar -tf /root/etcbackup.tar.gz
```

Notice karein ke hum **same command** use kar rahe hain.

Koi alag gzip option dene ki zarurat nahi.

---

# 🔬 Lab 3 – bzip2 Compressed Archive Ke Contents Dekhna

Suppose archive hai:

```text
etcbackup.tar.bz2
```

Command:

```bash
tar -tf /root/etcbackup.tar.bz2
```

Yahan bhi wahi command use hoti hai.

---

# 🔬 Lab 4 – xz Compressed Archive Ke Contents Dekhna

Suppose archive hai:

```text
etcbackup.tar.xz
```

Command:

```bash
tar -tf /root/etcbackup.tar.xz
```

Ek baar phir wahi command kaam karti hai.

---

# TAR Ko Compression Type Kaise Pata Chalta Hai?

`tar` utility ki sab se achhi features mein se ek ye hai ke ye archive ki compression type automatically pehchan leti hai.

Jab aap ye command chalate hain:

```bash
tar -tf archive_name
```

To TAR khud check karta hai ke archive:

- Normal TAR hai.
- gzip compressed hai.
- bzip2 compressed hai.
- xz compressed hai.

Aap ko alag se compression option batane ki zarurat nahi parti.

---

# Supported Archive Formats

Ek hi command in tamam archive types ke liye kaam karti hai.

| Archive Type | Extension | Command |
|--------------|-----------|---------|
| TAR | `.tar` | `tar -tf archive.tar` |
| gzip | `.tar.gz` | `tar -tf archive.tar.gz` |
| bzip2 | `.tar.bz2` | `tar -tf archive.tar.bz2` |
| xz | `.tar.xz` | `tar -tf archive.tar.xz` |

---

# Practical Example

Suppose `/root` directory ke andar ye files mojood hain:

```text
etcbackup.tar
etcbackup.tar.gz
etcbackup.tar.bz2
etcbackup.tar.xz
```

Aap in tamam archives ke contents dekh sakte hain:

```bash
tar -tf /root/etcbackup.tar
```

```bash
tar -tf /root/etcbackup.tar.gz
```

```bash
tar -tf /root/etcbackup.tar.bz2
```

```bash
tar -tf /root/etcbackup.tar.xz
```

Har dafa command bilkul same rahegi.

---

# Ye Feature Itni Useful Kyun Hai?

Suppose aap ko kisi doosre Linux server se backup archive mili hai.

Extract karne se pehle aap verify kar sakte hain:

- Kya is mein sahi files hain?
- Backup complete hai?
- Ye wahi archive hai jo mujhe chahiye?

Is tarah waqt bhi bachta hai aur ghalat archive extract karne se bhi bach jate hain.

---

# 🧪 Practice Exercises

---

## Exercise 1

Normal TAR archive ke contents dekhein.

```bash
tar -tf /root/etcbackup.tar
```

---

## Exercise 2

gzip archive ke contents dekhein.

```bash
tar -tf /root/etcbackup.tar.gz
```

---

## Exercise 3

bzip2 archive ke contents dekhein.

```bash
tar -tf /root/etcbackup.tar.bz2
```

---

## Exercise 4

xz archive ke contents dekhein.

```bash
tar -tf /root/etcbackup.tar.xz
```

---

## Exercise 5

Tamam commands ka output compare karein.

Observe karein ke command syntax har archive type ke liye bilkul same hai.

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Aap bhool gaye ke backup archive ke andar kaunsi files hain.

Solution:

```bash
tar -tf archive.tar
```

---

### Scenario 2

Kisi ne aap ko compressed archive bheji hai.

Aap ko nahi pata ke wo:

- gzip hai.
- bzip2 hai.
- xz hai.

Sirf ye command chalayein:

```bash
tar -tf archive_name
```

TAR khud compression type detect kar lega.

---

### Scenario 3

Backup restore karne se pehle usay verify karna hai.

Command:

```bash
tar -tf archive_name
```

Hamesha archive ko extract karne se pehle verify karna ek achhi practice hai.

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `tar -tf archive.tar` | TAR archive ke contents dekho |
| `tar -tf archive.tar.gz` | gzip archive ke contents dekho |
| `tar -tf archive.tar.bz2` | bzip2 archive ke contents dekho |
| `tar -tf archive.tar.xz` | xz archive ke contents dekho |

---

# 📖 Key Takeaways

- `tar -tf` archive ko extract kiye baghair us ke contents dikhata hai.
- Ek hi command:
  - `.tar`
  - `.tar.gz`
  - `.tar.bz2`
  - `.tar.xz`

sab ke liye kaam karti hai.

- TAR automatically compression method detect kar leta hai.
- Archive ko extract karne se pehle usay verify karna best practice hai.
- Archive ke contents list karna backup verification ka important hissa hai.

---

# 💡 Yaad Rakhein

> **TAR Archive ko ek band carton ya storage box ki tarah samjhein.**
>
> Box ko kholne se pehle aap us ke upar lagi packing list dekh sakte hain.
>
> ```bash
> tar -tf archive_name
> ```
>
> Ye command aap ko archive ke andar mojood tamam files dikha deti hai bina unhein extract kiye.
>
> Sab se achhi baat ye hai ke **ye ek hi command tamam common TAR archive formats (.tar, .tar.gz, .tar.bz2 aur .tar.xz) ke liye kaam karti hai.**