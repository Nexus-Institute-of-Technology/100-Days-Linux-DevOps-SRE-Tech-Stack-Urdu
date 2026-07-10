# MODULE 02/07 – Practice Lab: Command-Line File Management
> **Hands-on Practice Lab – `mkdir` Command Se Directories Banana (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `mkdir` command ki madad se directories kaise create karni hain.
- `cd` command se directories ke darmiyan navigate kaise karna hai.
- Ek hi command se multiple directories kaise create karni hain.
- Agar directory pehle se mojood ho to kya hota hai.
- `-p` option ki madad se parent aur nested directories kaise create karni hain.
- `pwd` aur `ls` commands se directory creation ko verify kaise karna hai.

---

# 📖 Introduction

Har Linux System Administrator ke liye sab se pehli aur important skills mein se ek **file aur directory management** hai.

Linux files aur directories ko manage karne ke liye bohot si command-line utilities provide karta hai.

Sab se zyada use hone wali commands ye hain:

- `mkdir` – Directory create karna
- `cd` – Directory change karna
- `pwd` – Current working directory dekhna
- `ls` – Directory ke contents dekhna

Is practice lab mein hum **mkdir** command ko detail mein seekhenge.

---

# `mkdir` Kya Hai?

`mkdir` ka matlab hai:

> **Make Directory**

Ye command use hoti hai:

- Ek single directory create karne ke liye
- Multiple directories create karne ke liye
- Parent directories create karne ke liye
- Nested subdirectories create karne ke liye

---

# Basic Syntax

```bash
mkdir directory_name
```

Example:

```bash
mkdir test
```

Ye command current working directory ke andar **test** naam ki directory create karegi.

---
# Lab shuru karnay say pehlay AGAR dev1 user nahi hai to dev1 naya user banain
```bash
useradd dev1
passwd abc
```

# 🔬 Lab 1 – Current Location Dekhein

Directory create karne se pehle apni current location check karein.

```bash
cd /home/dev1
pwd
```

Example Output:

```text
/home/dev1
```

---

# 🔬 Lab 2 – Existing Directories Ki List Dekhein

Current directory ke contents dekhein (kuh mashur ls commands ko dekhain!)

```bash
ls 
ls -l
ls -ltr
ls -al
ls -ali
```

Observe karein ke pehle se kaunsi files aur directories mojood hain.

---

# 🔬 Lab 3 – Ek Single Directory Create Karein

`test` naam ki directory banayein.

```bash
mkdir test
```

Verify karein:

```bash
ls
```

Expected Output:

```text
test
```

---

# 🔬 Lab 4 – Directory Ke Andar Jayein

Nayi create ki hui directory ke andar move karein.

```bash
cd test
```

Apni location verify karein.

```bash
pwd
```

Example Output:

```text
/home/techstart/test
```

Is se confirm hota hai ke aap successfully directory change kar chuke hain.

---

# 🔬 Lab 5 – Parent Directory Mein Wapas Jayein

Previous (parent) directory mein wapas aane ke liye:

```bash
cd ..
```

Verify karein:

```bash
pwd
```

---

# 🔬 Lab 6 – Ek Hi Command Se Multiple Directories Banayein

`mkdir` command ek hi command mein multiple directories create kar sakti hai.

Example:

```bash
mkdir projectX projectY projectZ
```

Verify karein:

```bash
ls
```

Expected Output:

```text
projectX
projectY
projectZ
```

Ye teenon directories ek hi location par create hongi.

---

# 🔬 Lab 7 – Existing Directory Dobarah Create Karne Ki Koshish

Agar directory pehle se mojood ho.

Command:

```bash
mkdir projectX
```

Expected Output:

```text
mkdir: cannot create directory 'projectX': File exists
```

Linux duplicate directory create nahi karta aur error display karta hai.

---

# Error Ko Samjhein

Agar directory pehle se exist karti ho:

- Directory dobara create nahi hoti.
- Existing data change nahi hota.
- Linux sirf error message show karta hai.

---

# 🔬 Lab 8 – Parent Aur Nested Directories Create Karein

Kabhi kabhi humein ek hi command mein multiple level ki directories create karni hoti hain.

Is ke liye `-p` option use hota hai.

Syntax:

```bash
mkdir -p parent/subdirectory/subdirectory
```

Example:

```bash
mkdir -p project/app/guacamole
```

Ye command ye structure create karegi:

```text
project
└── app
    └── guacamole
```

Agar parent directory pehle se mojood na ho to Linux automatically tamam required directories create kar deta hai.

---

# 🔬 Lab 9 – Parent Directory Verify Karein

Current location dekhein.

```bash
pwd
```

Directories ki list dekhein.

```bash
ls
```

Aap ko output mein ye nazar aana chahiye:

```text
project
```

---

# 🔬 Lab 10 – Nested Directories Mein Navigate Karein

Parent directory ke andar jayein.

```bash
cd project
```

Location verify karein.

```bash
pwd
```

Contents dekhein.

```bash
ls
```

Expected Output:

```text
app
```

Ab agli directory ke andar jayein.

```bash
cd app
```

Location verify karein.

```bash
pwd
```

Contents dekhein.

```bash
ls
```

Expected Output:

```text
guacamole
```

Ye confirm karta hai ke tamam nested directories successfully create ho chuki hain.

---

# Directory Structure

Lab complete karne ke baad aap ki directory structure kuch is tarah hogi:

```text
/home/techstart
│
├── test
├── projectX
├── projectY
├── projectZ
└── project
    └── app
        └── guacamole
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Apni current location display karein.

```bash
pwd
```

---

## Exercise 2

Directory ke contents list karein.

```bash
ls
```

---

## Exercise 3

`training` naam ki directory create karein.

```bash
mkdir training
```

---

## Exercise 4

Us directory ke andar enter karein.

```bash
cd training
```

---

## Exercise 5

Parent directory mein wapas aayein.

```bash
cd ..
```

---

## Exercise 6

Teen directories create karein.

```bash
mkdir linux aws azure
```

---

## Exercise 7

Nested directories create karein.

```bash
mkdir -p project/app/guacamole
```

---

## Exercise 8

Har directory ke andar navigate karein aur apni location verify karein.

Commands:

```bash
cd
```

Aur

```bash
pwd
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Directory pehle se mojood hai.

Command:

```bash
mkdir projectX
```

Linux error show karega:

```text
File exists
```

---

### Scenario 2

Ek hi command mein multiple directories create karni hain.

Command:

```bash
mkdir dir1 dir2 dir3
```

---

### Scenario 3

Multiple nested directories create karni hain.

Command:

```bash
mkdir -p company/IT/Linux
```

---

### Scenario 4

Aap ko nahi pata aap is waqt kis directory mein hain.

Command:

```bash
pwd
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `mkdir test` | Ek directory create kare |
| `mkdir dir1 dir2 dir3` | Multiple directories create kare |
| `mkdir -p project/app/guacamole` | Parent aur nested directories create kare |
| `cd directory` | Directory change kare |
| `cd ..` | Parent directory mein wapas jaye |
| `pwd` | Current location show kare |
| `ls` | Directory ke contents show kare |

---

# 📖 Key Takeaways

- `mkdir` directories create karne ke liye use hota hai.
- Ek hi command se multiple directories create ki ja sakti hain.
- `-p` option automatically parent aur nested directories create karta hai.
- `cd` current directory change karta hai.
- `pwd` current working directory dikhata hai.
- `ls` verify karta hai ke directories successfully create hui hain.

---

# 💡 Yaad Rakhein

> **Directories ko office ke filing cabinet ke folders ki tarah samjhein.**
>
> - `mkdir` naya folder banata hai.
> - `cd` us folder ko open karta hai.
> - `pwd` batata hai ke aap is waqt kis folder ke andar hain.
> - `ls` current folder ke andar mojood tamam cheezen dikhata hai.
>
> **In bunyadi commands par achhi command hasil karna Linux File Management ki strong foundation hai.**