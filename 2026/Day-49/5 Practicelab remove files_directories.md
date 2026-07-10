# MODULE 02/07 – Practice Lab: Removing Files and Directories
> **Hands-on Practice Lab – `rm` Aur `rmdir` Commands Se Files Aur Directories Delete Karna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `rm` command ki madad se files delete karna.
- `rmdir` command ki madad se empty directories delete karna.
- `rm -r` command se directories ko recursively delete karna.
- `rm -rf` command se forcefully directories aur un ke contents delete karna.
- `rm` aur `rmdir` ke darmiyan farq samajhna.
- Ye samajhna ke `rm -rf` command bohot dangerous kyun hai.

---

# 📖 Introduction

Linux mein file management ka ek important hissa unnecessary files aur directories ko remove karna bhi hai.

Linux is kaam ke liye do commonly use hone wali commands provide karta hai:

- `rm` – Files remove karne ke liye (aur options ke saath directories bhi)
- `rmdir` – Sirf empty directories remove karne ke liye

By default:

- `rm` sirf **files** remove karta hai.
- `rmdir` sirf **empty directories** remove karta hai.

Agar directory ke andar files ya subdirectories mojood hon to usay delete karne ke liye `rm -r` use karna zaroori hai.

---

# `rm` Command Kya Hai?

`rm` ka matlab hai:

> **Remove**

Ye command use hoti hai:

- Files delete karne ke liye.
- Directories delete karne ke liye (options ke saath).

---

# Basic Syntax

File delete karne ke liye:

```bash
rm filename
```

Directory recursively delete karne ke liye:

```bash
rm -r directory_name
```

Directory ko forcefully delete karne ke liye:

```bash
rm -rf directory_name
```

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

---

# 🔬 Lab 2 – File Delete Karein

Suppose current directory mein:

```text
file1
```

naam ki file mojood hai.

Usay delete karein.

```bash
rm file1
```

System confirmation maang sakta hai.

Example:

```text
rm: remove regular file 'file1'?
```

Type karein:

```text
y
```

Aur **Enter** press karein.

File delete ho jayegi.

---

# 🔬 Lab 3 – File Verify Karein

Directory ke contents dekhein.

```bash
ls
```

Delete ki hui file ab list mein nazar nahi aani chahiye.

---

# 🔬 Lab 4 – Ek Directory Create Karein

Nayi directory create karein.

```bash
mkdir test
```

Verify karein.

```bash
ls
```

---

# 🔬 Lab 5 – Directory Delete Karne Ki Koshish Karein

Ab directory ko delete karne ki koshish karein.

```bash
rm test
```

Expected Output:

```text
rm: cannot remove 'test': Is a directory
```

By default `rm` directories ko delete nahi karta.

---

# 🔬 Lab 6 – Directory Recursively Delete Karein

Directory delete karne ke liye recursive option use karein.

```bash
rm -r test
```

System confirmation maang sakta hai.

Type karein:

```text
y
```

Directory delete ho jayegi.

---

# `-r` Option Kya Karta Hai?

`-r` ka matlab hai:

> **Recursive**

Ye Linux ko batata hai:

- Directory ke andar jao.
- Andar ki tamam files delete karo.
- Tamam subdirectories delete karo.
- Aakhir mein main directory delete karo.

---

# 🔬 Lab 7 – Empty Directory `rmdir` Se Delete Karein

Ek aur directory create karein.

```bash
mkdir test1
```

Ab usay delete karein.

```bash
rmdir test1
```

Ye directory successfully delete ho jayegi kyun ke ye empty hai.

---

# `rmdir` Command Kya Hai?

`rmdir` command sirf:

> **Empty directories** delete karti hai.

Agar directory ke andar files hon to ye command fail ho jati hai.

---

# 🔬 Lab 8 – Non-Empty Directory Create Karein

Ek aur directory create karein.

```bash
mkdir test2
```

Us ke andar jayein.

```bash
cd test2
```

Teen files create karein.

```bash
touch file1 file2 file3
```

Parent directory mein wapas aayein.

```bash
cd ..
```

---

# 🔬 Lab 9 – Non-Empty Directory Delete Karne Ki Koshish Karein

Command chalayein.

```bash
rmdir test2
```

Expected Output:

```text
rmdir: failed to remove 'test2': Directory not empty
```

`rmdir` sirf empty directories delete karta hai.

---

# 🔬 Lab 10 – Non-Empty Directory Delete Karein

Ab use recursively delete karein.

```bash
rm -r test2
```

System confirmation maangega:

- Directory ke andar jana hai?
- Har file delete karni hai?
- Directory delete karni hai?

Har prompt par type karein:

```text
y
```

Directory aur us ke tamam contents delete ho jayenge.

---

# 🔬 Lab 11 – Forcefully Directory Delete Karein

Ek aur directory create karein.

```bash
mkdir test3
```

Directory ke andar jayein.

```bash
cd test3
```

Teen files create karein.

```bash
touch file1 file2 file3
```

Parent directory mein wapas aayein.

```bash
cd ..
```

Ab bina kisi confirmation ke directory delete karein.

```bash
rm -rf test3
```

Directory aur us ke andar ki tamam files foran delete ho jayengi.

Koi confirmation nahi poochi jayegi.

---

# `-f` Option Kya Hai?

`-f` ka matlab hai:

> **Force**

Ye Linux ko batata hai:

- Confirmation mat poochho.
- Warnings ignore karo.
- Seedha delete kar do.

Jab `-r` aur `-f` dono saath use hote hain:

```bash
rm -rf directory
```

To Linux tamam contents bina poochhe delete kar deta hai.

---

# ⚠️ Warning – `rm -rf`

Ye Linux ki sab se powerful aur dangerous commands mein se ek hai.

```bash
rm -rf directory_name
```

Agar ghalti se wrong directory delete kar di to data permanently delete ho sakta hai.

Command chalane se pehle directory ka naam hamesha dobarah check karein.

---

# `rm` vs `rmdir`

| Command | Kaam |
|----------|------|
| `rm file` | File delete kare |
| `rm -r directory` | Directory aur us ke contents recursively delete kare |
| `rm -rf directory` | Forcefully directory delete kare |
| `rmdir directory` | Sirf empty directory delete kare |

---

# Directory Structure Example

Delete karne se pehle:

```text
test3
├── file1
├── file2
└── file3
```

Command:

```bash
rm -rf test3
```

Result:

Directory aur us ke tamam files permanently delete ho jayengi.

---

# 🧪 Practice Exercises

---

## Exercise 1

Teen files create karein.

```bash
touch file1 file2 file3
```

Ek file delete karein.

```bash
rm file1
```

---

## Exercise 2

Ek empty directory create karein.

```bash
mkdir backup
```

Usay delete karein.

```bash
rmdir backup
```

---

## Exercise 3

Ek directory aur us ke andar files create karein.

```bash
mkdir data
cd data
touch a b c
cd ..
```

Recursive delete karein.

```bash
rm -r data
```

---

## Exercise 4

Ek aur directory create karein.

```bash
mkdir temp
cd temp
touch x y z
cd ..
```

Forcefully delete karein.

```bash
rm -rf temp
```

---

## Exercise 5

Deletion verify karein.

```bash
ls
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Aap directory delete karte hain.

```bash
rm directory
```

Error:

```text
Is a directory
```

Solution:

```bash
rm -r directory
```

---

### Scenario 2

Aap `rmdir` use karte hain.

```bash
rmdir directory
```

Error:

```text
Directory not empty
```

Solution:

```bash
rm -r directory
```

---

### Scenario 3

Aap confirmation nahi chahte.

Command:

```bash
rm -rf directory
```

Ye command bohot ehtiyaat se use karein.

---

### Scenario 4

Ghalti se wrong directory delete ho gayi.

```bash
rm -rf
```

Linux mein iska koi built-in undo command nahi hota.

Hamesha command chalane se pehle path verify karein.

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `rm file` | File delete kare |
| `rm -r directory` | Directory recursively delete kare |
| `rm -rf directory` | Forcefully directory delete kare |
| `rmdir directory` | Sirf empty directory delete kare |
| `mkdir test` | Directory create kare |
| `touch file` | Empty file create kare |
| `ls` | Directory ke contents show kare |

---

# 📖 Key Takeaways

- `rm` by default files delete karta hai.
- `rm -r` directories aur un ke tamam contents recursively delete karta hai.
- `rm -rf` bina confirmation ke delete karta hai.
- `rmdir` sirf empty directories delete karta hai.
- `rm -rf` chalane se pehle hamesha directory verify karein.
- `-f` option bohot powerful hai aur deleted data aasani se recover nahi hota.

---

# 💡 Yaad Rakhein

> **`rm` command ko paper shredder ki tarah samjhein.**
>
> - `rm` file delete karta hai.
> - `rm -r` poori directory aur us ke andar ki tamam files delete karta hai.
> - `rm -rf` bina poochhe foran delete kar deta hai.
> - `rmdir` sirf empty directory delete karta hai.
>
> **Golden USOOL Yaad Rakhein:**
>
> ```text
> File Delete             = rm file
>
> Empty Directory Delete  = rmdir directory
>
> Directory Delete        = rm -r directory
>
> Force Delete            = rm -rf directory
> ```
>
> **`rm -rf` command chalate waqt hamesha bohot zyada ehtiyaat karein, kyun ke ye data permanently delete kar deti hai.**