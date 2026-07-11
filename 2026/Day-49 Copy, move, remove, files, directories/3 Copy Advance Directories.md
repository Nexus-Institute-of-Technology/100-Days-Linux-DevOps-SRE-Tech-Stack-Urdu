# MODULE 02/07 – Practice Lab: Advanced Copy Operations with `cp`
> **Hands-on Practice Lab – Multiple Files Aur Directories Ko `cp` Command Se Copy Karna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- Ek hi command se multiple files copy karna.
- Relative Path aur Absolute Path ka istemal karna.
- Poori directory ko recursively copy karna.
- `cp -R` option ka use samajhna.
- `ls` command ke mukhtalif options se directory contents verify karna.
- `man` command ki madad se command ki documentation dekhna.

---

# 📖 Introduction

Pichlay practice lab mein hum ne seekha tha ke **`cp` command** ki madad se ek file ko copy kaise karte hain.

Is lab mein hum seekhenge:

- Multiple files ko ek hi command se copy karna.
- Relative aur Absolute Path ka istemal.
- Poori directory ko copy karna.
- `ls` command ke advanced options use karna.

Ye tamam commands RHCSA aur roz marrah Linux administration mein bohot zyada use hoti hain.

---

# 🔬 Lab 1 – Current Location Verify Karein

Sab se pehle current location check karein.

```bash
pwd
```

Directory ke contents dekhein.

```bash
ls
```

Suppose current directory mein ye files mojood hain:

```text
blockbuster1.ogg
blockbuster2.ogg
blockbuster3.ogg
projectX
projectY
projectZ
```

---

# 🔬 Lab 2 – Multiple Files Relative Path Se Copy Karein

Ab hum teeno files ko ek hi command se **projectZ** directory mein copy karenge.

Command:

```bash
cp blockbuster1.ogg blockbuster2.ogg blockbuster3.ogg projectZ/
```

### Explanation

| Part | Matlab |
|------|---------|
| `cp` | Copy command |
| `blockbuster1.ogg` | Source file |
| `blockbuster2.ogg` | Source file |
| `blockbuster3.ogg` | Source file |
| `projectZ/` | Destination directory |

Ye command teeno files ko ek hi dafa **projectZ** ke andar copy kar degi.

---

# 🔬 Lab 3 – Verify the Copy

Destination directory mein jayein.

```bash
cd projectZ
```

Files verify karein.

```bash
ls
```

Expected Output:

```text
blockbuster1.ogg
blockbuster2.ogg
blockbuster3.ogg
```

Ab wapas previous directory mein aayein.

```bash
cd ..
```

---

# 🔬 Lab 4 – Absolute Path Se Multiple Files Copy Karein

Ab wahi files **Absolute Path** use karke **projectX** directory mein copy karein.

Command:

```bash
cp blockbuster1.ogg blockbuster2.ogg blockbuster3.ogg /home/techstart/test/projectX/
```

Yahan hum ne poora path use kiya hai.

Ye **Absolute Path** hai.

---

# Relative Path vs Absolute Path

## Relative Path

Current location se path likha jata hai.

Example:

```bash
cp blockbuster1.ogg blockbuster2.ogg blockbuster3.ogg projectZ/
```

---

## Absolute Path

Root (`/`) se poora path likha jata hai.

Example:

```bash
cp blockbuster1.ogg blockbuster2.ogg blockbuster3.ogg /home/techstart/test/projectX/
```

Dono tareeqay bilkul sahi hain.

---

# 🔬 Lab 5 – Directory Copy Karna

Ab hum poori directory copy karenge.

Suppose:

```text
projectX
```

ko

```text
projectK
```

ke naam se copy karna hai.

Command:

```bash
cp projectX projectK
```

Expected Output:

```text
cp: -r not specified; omitting directory 'projectX'
```

Ye error is liye aayi kyun ke **cp by default directories copy nahi karta.**

---

# Directory Copy Ke Liye `-R` Option

Directories copy karne ke liye:

```bash
cp -R projectX projectK
```

### Explanation

| Option | Matlab |
|---------|---------|
| `-R` | Recursive Copy |

Ye command:

- `projectK` naam ki nayi directory create karegi.
- `projectX` ke andar mojood tamam files aur subdirectories ko copy karegi.

---

# 🔬 Lab 6 – Directory Verify Karein

Directory ke contents dekhne ke liye:

```bash
ls -la projectK
```

Expected Output:

```text
blockbuster1.ogg
blockbuster2.ogg
blockbuster3.ogg
```

Ye confirm karta hai ke directory successfully copy ho gayi hai.

---

# 🔬 Lab 7 – Kisi Directory Ke Contents Dekhna

Kisi bhi directory ke detailed contents dekhne ke liye:

```bash
ls -la projectX
```

Ya

```bash
ls -la projectK
```

Ye command:

- Files
- Hidden Files
- Permissions
- Ownership
- Size
- Date

sab kuch display karti hai.

---

# 🔬 Lab 8 – Nested Directories Verify Karein

Suppose:

```text
project
└── app
    └── guacamole
```

Directory ke contents dekhne ke liye:

```bash
ls -la project
```

Output:

```text
app
```

Aur:

```bash
ls -la project/app
```

Output:

```text
guacamole
```

---

# 🔬 Lab 9 – Command Ki Documentation Dekhna

Kisi bhi command ki complete documentation dekhne ke liye:

```bash
man ls
```

Ya

```bash
man cp
```

Yahan aap ko command ke tamam options aur examples milenge.

---

# 🔬 Lab 10 – Recursive Listing

Agar aap tamam directories aur un ke andar mojood files ek hi command se dekhna chahte hain.

Command:

```bash
ls -lR
```

### Explanation

| Option | Matlab |
|---------|---------|
| `-l` | Long Listing |
| `-R` | Recursive Listing |

Agar output bohot lambi ho to:

```bash
ls -lR | less
```

`less` command output ko page by page show karti hai.

---

# Directory Structure

Lab complete hone ke baad structure kuch is tarah hogi:

```text
/home/dev1
│
├── blockbuster1.ogg
├── blockbuster2.ogg
├── blockbuster3.ogg
├── projectX
│   ├── blockbuster1.ogg
│   ├── blockbuster2.ogg
│   └── blockbuster3.ogg
├── projectK
│   ├── blockbuster1.ogg
│   ├── blockbuster2.ogg
│   └── blockbuster3.ogg
├── projectY
└── projectZ
    ├── blockbuster1.ogg
    ├── blockbuster2.ogg
    └── blockbuster3.ogg
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Teen files create karein.

```bash
touch file1 file2 file3
```

---

## Exercise 2

Ek hi command se sab files ko backup directory mein copy karein.

```bash
cp file1 file2 file3 backup/
```

---

## Exercise 3

Poori directory recursively copy karein.

```bash
cp -R backup backup_copy
```

---

## Exercise 4

Directory ke contents verify karein.

```bash
ls -la backup_copy
```

---

## Exercise 5

Recursive listing dekhein.

```bash
ls -lR | less
```

---

## Exercise 6

`cp` command ki documentation dekhein.

```bash
man cp
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Directory copy karte waqt error aati hai.

Command:

```bash
cp projectX projectK
```

Error:

```text
cp: -r not specified; omitting directory
```

Solution:

```bash
cp -R projectX projectK
```

---

### Scenario 2

Aap ko tamam directories aur un ke contents dekhne hain.

Command:

```bash
ls -lR
```

---

### Scenario 3

Output bohot lambi aa rahi hai.

Command:

```bash
ls -lR | less
```

---

### Scenario 4

Aap ko kisi command ke options yaad nahi.

Command:

```bash
man cp
```

Ya

```bash
man ls
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `cp file1 file2 dir/` | Multiple files copy kare |
| `cp -R dir1 dir2` | Directory recursively copy kare |
| `ls -la directory` | Directory ke detailed contents dekhe |
| `ls -lR` | Recursive listing kare |
| `ls -lR \| less` | Page by page recursive output dekhe |
| `man cp` | `cp` command ki documentation dekhe |
| `man ls` | `ls` command ki documentation dekhe |

---

# 📖 Key Takeaways

- `cp` ek hi command mein multiple files copy kar sakta hai.
- Relative aur Absolute Path dono use kiye ja sakte hain.
- Directories copy karne ke liye **`-R`** option zaroori hai.
- `ls -la` detailed listing dikhata hai.
- `ls -lR` recursively tamam directories aur files show karta hai.
- `man` command Linux commands ki complete documentation provide karti hai.

---

# 💡 Yaad Rakhein

> **`cp` command ko ek Photocopy Machine ki tarah samjhein.**
>
> - Ek file ki copy bana sakti hai.
> - Ek hi dafa multiple files ki copies bana sakti hai.
> - Poori directory ki bhi copy bana sakti hai (sirf `-R` ke saath).
>
> **Golden Rule Yaad Rakhein:**
>
> ```text
> File Copy = cp
> Directory Copy = cp -R
> ```
>
> **Agar directory copy karni ho to `-R` option kabhi mat bhooliye!**