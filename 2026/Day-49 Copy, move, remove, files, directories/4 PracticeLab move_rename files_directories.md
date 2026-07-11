# MODULE 02/07 – Practice Lab: Moving and Renaming Files Using `mv`
> **Hands-on Practice Lab – `mv` Command Se Files Move Aur Rename Karna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `mv` command ki madad se files ko rename karna.
- Files ko ek location se doosri location par move karna.
- Relative Path aur Absolute Path ka istemal karna.
- `ls` command ki madad se moved files ko verify karna.
- `mv` command ke dono main functions ko samajhna.

---

# 📖 Introduction

Linux mein **`mv` (Move)** command do bohot important kaamon ke liye use hoti hai:

1. Files ya directories ko ek location se doosri location par move karna.
2. Files ya directories ko rename karna.

`cp` command ke baraks, **`mv` command file ki copy create nahi karti**.

Ye original file ko hi:

- Nayi location par move karti hai.
- Ya us ka naam change kar deti hai.

Basic syntax:

```bash
mv source destination
```

Destination ke mutabiq ye command:

- File ko rename karegi.
- File ko move karegi.
- Ya ek hi waqt mein move aur rename dono karegi.

---

# `mv` Command Kya Hai?

`mv` ka matlab hai:

> **Move**

Ye command use hoti hai:

- Files move karne ke liye.
- Directories move karne ke liye.
- Files rename karne ke liye.
- Directories rename karne ke liye.

---

# Basic Syntax

```bash
mv source destination
```

Yahan:

- **Source** = Existing file ya directory.
- **Destination** = Naya filename ya destination directory.

---

# 🔬 Lab 1 – Current Location Verify Karein

Sab se pehle apni current location check karein.

```bash
pwd
```

Directory ke contents dekhein.

```bash
ls -ltr
```

Suppose current directory mein ye files mojood hain:

```text
newabc
pic1.jpg
pic2.jpg
videos/
```

---

# 🔬 Lab 2 – File Ko Rename Karein

Suppose aap is file:

```text
newabc
```

ka naam change karke

```text
abc
```

rakhna chahte hain.

Command:

```bash
mv newabc abc
```

Verify karein.

```bash
ls -ltr
```

Expected Output:

```text
abc
```

Ab file successfully rename ho chuki hai.

---

# File Rename Kaise Hota Hai?

Jab destination ek **naya filename** hota hai to `mv` command sirf filename change karti hai.

Example:

```bash
mv oldname newname
```

Is command se:

- Koi nayi file create nahi hoti.
- Sirf file ka naam badalta hai.

---

# 🔬 Lab 3 – Absolute Path Se File Move Karein

Suppose:

```text
pic1.jpg
```

ye file is location par mojood hai:

```text
/home/dev1/videos/
```

Aur aap ise move karna chahte hain:

```text
/tmp
```

Command:

```bash
mv /home/dev1/videos/pic1.jpg /tmp/
```

Yahan poora path likha gaya hai.

Ye **Absolute Path** hai.

---

# 🔬 Lab 4 – File Verify Karein

`/tmp` directory mein jayein.

```bash
cd /tmp
```

Contents dekhein.

```bash
ls -ltr
```

Expected Output:

```text
pic1.jpg
```

Ye confirm karta hai ke file successfully move ho chuki hai.

---

# 🔬 Lab 5 – Previous Directory Mein Wapas Aayein

Apni pehli location par wapas aane ke liye:

```bash
cd -
```

Ya:

```bash
cd /home/dev1
```

---

# 🔬 Lab 6 – Relative Path Se File Move Karein

Ab:

```text
pic2.jpg
```

ko Relative Path use karte hue move karein.

Example:

```bash
mv pic2.jpg documents/
```

Yahan destination current directory ke mutabiq likha gaya hai.

Isay **Relative Path** kehte hain.

---

# Relative Path vs Absolute Path

## Relative Path

Current working directory se path likha jata hai.

Example:

```bash
mv pic2.jpg documents/
```

---

## Absolute Path

Root (`/`) se poora path likha jata hai.

Example:

```bash
mv /home/dev1/videos/pic1.jpg /tmp/
```

Dono tareeqay bilkul sahi hain.

---

# 🔬 Lab 7 – Move Verify Karein

Destination directory mein jayein.

```bash
cd documents
```

Contents dekhein.

```bash
ls
```

Expected Output:

```text
pic2.jpg
```

Is se confirm hota hai ke file successfully move ho gayi hai.

---

# Directory Structure

### Move Karne Se Pehle

```text
/home/dev1
│
├── abc
├── pic2.jpg
├── videos
│   └── pic1.jpg
└── documents
```

---

### Move Karne Ke Baad

```text
/home/dev1
│
├── abc
├── videos
└── documents
    └── pic2.jpg

/tmp
└── pic1.jpg
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Do files create karein.

```bash
touch file1 file2
```

---

## Exercise 2

`file1` ka naam change karke:

```text
linux.txt
```

rakh dein.

```bash
mv file1 linux.txt
```

---

## Exercise 3

Ek directory create karein.

```bash
mkdir backup
```

---

## Exercise 4

`file2` ko backup directory mein move karein.

```bash
mv file2 backup/
```

---

## Exercise 5

Move verify karein.

```bash
ls backup
```

---

## Exercise 6

Absolute Path use karte hue file move karein.

Example:

```bash
mv /home/dev1/linux.txt /tmp/
```

---

## Exercise 7

Apni home directory mein wapas aayein.

```bash
cd ~
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Source file mojood nahi.

Command:

```bash
mv file1 backup/
```

Error:

```text
No such file or directory
```

Sab se pehle verify karein ke file waqai mojood hai.

---

### Scenario 2

Destination directory mojood nahi.

Command:

```bash
mv file1 storage/
```

Linux error show karega ke destination directory exist nahi karti.

---

### Scenario 3

Ghalti se ghalat file rename kar di.

Dobarah rename kar dein.

Example:

```bash
mv wrongname correctname
```

---

### Scenario 4

Aap ko yaqeen nahi ke file move hui hai ya nahi.

Verify karein:

```bash
ls
```

Ya

```bash
ls -ltr
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `mv old new` | File rename kare |
| `mv file directory/` | File ko doosri directory mein move kare |
| `mv /path/file /tmp/` | Absolute Path se file move kare |
| `mv file documents/` | Relative Path se file move kare |
| `pwd` | Current directory show kare |
| `ls -ltr` | Directory ke contents show kare |

---

# 📖 Key Takeaways

- `mv` command files aur directories ko move aur rename karne ke liye use hoti hai.
- `cp` ke baraks, `mv` duplicate copy create nahi karti.
- Relative aur Absolute Path dono use kiye ja sakte hain.
- `ls` command se hamesha verify karein ke file successfully move hui hai.
- Destination ke mutabiq `mv` command rename bhi karti hai aur move bhi.

---

# 💡 Yaad Rakhein

> **`mv` command ko is tarah samjhein jaise aap ek asli file ko ek folder se doosre folder mein le ja rahe hon.**
>
> - Original file ki koi copy nahi banti.
> - Sirf us ki location ya naam change hota hai.
>
> **Golden Rule Yaad Rakhein:**
>
> ```text
> File Rename = mv oldname newname
>
> File Move = mv file destination/
> ```
>
> **`cp` copy banata hai, jab ke `mv` original file ko hi move karta hai.**