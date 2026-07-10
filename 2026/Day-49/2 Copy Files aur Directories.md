# MODULE 02/07 – Practice Lab: Copy Files and Directories
> **Hands-on Practice Lab – `cp` Command Se Files Aur Directories Copy Karna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `cp` command ki madad se files copy karna.
- File ko current directory mein naye naam ke saath copy karna.
- File ko kisi doosri directory mein copy karna.
- Relative Path aur Absolute Path ka istemal karna.
- `touch` command se empty files create karna.
- Copy ki hui files ko verify karna.

---

# 📖 Introduction

Linux mein files aur directories ko copy karna roz marrah ka bohot important kaam hai.

Is kaam ke liye Linux **`cp` (Copy)** command provide karta hai.

`cp` command ki madad se aap:

- Ek file ki copy bana sakte hain.
- File ko kisi doosri directory mein bhej sakte hain.
- Ek hi file ko naye naam ke saath duplicate kar sakte hain.
- Directories ko bhi recursively copy kar sakte hain.

Is practice lab mein hum file copying ko step by step seekhenge.

---

# `cp` Kya Hai?

`cp` ka matlab hai:

> **Copy**

Ye command files aur directories ki copy banane ke liye use hoti hai.

---

# Basic Syntax

```bash
cp source destination
```

Yahan:

- **Source** = Jis file ko copy karna hai.
- **Destination** = Jahan copy rakhni hai.

---

# Lab shuru karnay say pehlay AGAR dev1 user nahi hai to dev1 naya user banain
```bash
useradd dev1
passwd abc
```

# 🔬 Lab 1 – Current Location Check Karein

Sab se pehle apni current location dekhein.

```bash
pwd
```

Agar zarurat ho to parent directory mein wapas jayein.

```bash
cd /home/dev1
```

Check karain:

```bash
ls -l
```

Ab current location verify karein.

```bash
pwd
```

---

# 🔬 Lab 2 – Empty Files Create Karein

Do empty files create karein.

```bash
touch blockbuster1.ogg
```

Aur:

```bash
touch blockbuster2.ogg
```

Ab files verify karein.

```bash
ls
```

Expected Output:

```text
blockbuster1.ogg
blockbuster2.ogg
```

---

# 🔬 Lab 3 – File Ko Naye Naam Se Copy Karein

Ab `blockbuster1.ogg` ki ek copy current directory mein hi naye naam ke saath banayein.

Command:

```bash
cp blockbuster1.ogg blockbuster3.ogg
```

Verify karein.

```bash
ls
```

Expected Output:

```text
blockbuster1.ogg
blockbuster2.ogg
blockbuster3.ogg
```

Ab `blockbuster3.ogg` bilkul original file ki copy hai.

---

# File Copy Hone Par Kya Hota Hai?

`cp` command:

- Original file ko change nahi karti.
- Nayi identical file create karti hai.
- Dono files ka content bilkul same hota hai.

Sirf filename alag hota hai.

---

# 🔬 Lab 4 – Destination Directory Samjhein

Suppose hamari directory structure kuch is tarah hai:

```text
project
└── app
    └── guacamole
```

Aur humein:

```text
blockbuster2.ogg
```

ko

```text
project/app/guacamole
```

ke andar copy karna hai.

---

# 🔬 Lab 5 – Relative Path Se File Copy Karein

Current location se relative path use karte hue:

```bash
cp blockbuster2.ogg city/Delhi/Dwarka/
```

Ye **Relative Path** hai.

Kyun?

Kyun ke hum ne current directory se required path likha hai.

Verify karein.

```bash
cd projecct/app/quacamole
```

```bash
ls
```

Expected Output:

```text
blockbuster2.ogg
```

File successfully copy ho chuki hai.

---

# 🔬 Lab 6 – Previous Location Mein Wapas Jayein

Teen level wapas aane ke liye:

```bash
cd ../../..
```

Ya step by step:

```bash
cd ..
cd ..
cd ..
```

Verify:

```bash
pwd
```

---

# 🔬 Lab 7 – Absolute Path Se File Copy Karein

Ab `blockbuster3.ogg` ko same destination mein **Absolute Path** use karke copy karein.

Command:

```bash
cp blockbuster3.ogg /home/techstart/city/Delhi/Dwarka/
```

Yahan hum ne poora path likha hai.

Isay **Absolute Path** kehte hain.

---

# Relative Path Aur Absolute Path

## Relative Path

Current directory se path likha jata hai.

Example:

```bash
cp blockbuster2.ogg project/app/guacamole
```

---

## Absolute Path

Root (`/`) se poora path likha jata hai.

Example:

```bash
cp blockbuster3.ogg /home/dev1/project/app/guacamole
```

Dono commands bilkul sahi hain.

---

# 🔬 Lab 8 – Existing File Ko Dobarah Copy Karna

Agar destination par file pehle se mojood ho aur aap dobarah copy karein.

Example:

```bash
cp blockbuster3.ogg /home/dev1/project/app/guacamole
```

Agar file already mojood hai to Linux usay overwrite karega ya overwrite confirmation maang sakta hai (system configuration par depend karta hai).

---

# Directory Structure

Lab complete hone ke baad structure kuch is tarah hoga: (tree command istimaal karain tree -d /home/dev1/)

```text
/home/dev1
│
├── blockbuster1.ogg
├── blockbuster2.ogg
├── blockbuster3.ogg
└── project
    └── app
        └── guacamole
            ├── blockbuster2.ogg
            └── blockbuster3.ogg
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Do empty files create karein.

```bash
touch movie1 movie2
```

---

## Exercise 2

`movie1` ki copy `movie3` ke naam se banayein.

```bash
cp movie1 movie3
```

---

## Exercise 3

Nested directory create karein.

```bash
mkdir -p backup/files
```

---

## Exercise 4

`movie2` ko relative path use karte hue copy karein.

```bash
cp movie2 backup/files/
```

---

## Exercise 5

`movie3` ko absolute path use karte hue copy karein.

```bash
cp movie3 /home/dev1/backup/files/
```

---

## Exercise 6

Destination directory mein ja kar verify karein.

```bash
cd backup/files
```

```bash
ls
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Source file nahi mil rahi.

Command:

```bash
cp movie1 backup/
```

Error:

```text
No such file or directory
```

Check karein ke file waqai current directory mein mojood hai.

---

### Scenario 2

Destination path ghalat hai.

Command:

```bash
cp movie1 project/xyz/
```

Linux error show karega ke destination exist nahi karti.

---

### Scenario 3

Aap confuse hain ke Relative Path use karna hai ya Absolute Path.

Yaad rakhein:

- Current location se path likhein → Relative Path
- Root (`/`) se poora path likhein → Absolute Path

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `touch file1` | Empty file create kare |
| `cp file1 file2` | File ki copy naye naam se banaye |
| `cp file project/app/` | Relative path se copy kare |
| `cp file /home/dev1/project/app/` | Absolute path se copy kare |
| `pwd` | Current location show kare |
| `ls` | Files aur directories ki list dekhe |

---

# 📖 Key Takeaways

- `cp` command files aur directories ki copy banane ke liye use hoti hai.
- Source aur Destination dono specify karna zaroori hota hai.
- Relative aur Absolute Path dono use kiye ja sakte hain.
- `touch` command empty files create karti hai.
- `ls` aur `pwd` commands verification ke liye bohot useful hain.

---

# 💡 Yaad Rakhein

> **`cp` command ko Photocopy Machine ki tarah samjhein.**
>
> - Original file apni jagah rehti hai.
> - `cp` us ki ek bilkul same copy bana deta hai.
> - Aap copy ko usi folder mein naye naam se bhi rakh sakte hain ya kisi doosri directory mein bhi bhej sakte hain.
>
> **Simple Formula Yaad Rakhein:**
>
> ```text
> cp Source Destination
> ```
>
> **Source = Jis file ki copy banani hai**
>
> **Destination = Jahan copy rakhni hai**