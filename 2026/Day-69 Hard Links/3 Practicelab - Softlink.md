# MODULE 10 – Practice Lab: Soft Links
> **Hands-on Practice Lab – Linux Mein Symbolic (Soft) Links Banana aur Samajhna**

---

# 🎯 Lab Objectives

Is lab ke end tak aap seekh jayenge:

- `ln -s` command se Symbolic (Soft) Link banana.
- Inode numbers ki madad se Soft Link verify karna.
- Directory listing mein Soft Link ko identify karna.
- Samajhna ke original file delete hone ke baad kya hota hai.
- Same directory mein Soft Link create karna.
- Different directories mein Soft Link create karna.
- Absolute Path ki importance ko samajhna.
- Different file systems ke across Soft Link create karna.
- Soft Link aur Hard Link ka comparison karna.

---

# 📖 Introduction

Pichle lab mein hum ne **Hard Links** banana seekha tha.

Is lab mein hum **Symbolic Link (Soft Link)** create karenge aur uska behavior practically samjhenge.

Hard Link ke baraks, Soft Link file ke inode ko directly point nahi karta.

Balke Soft Link sirf **original file ka path** store karta hai.

Soft Link ko aap Windows Shortcut ya Google Maps ke location pin ki tarah samajh sakte hain.

---

# 🔬 Lab Scenario

Suppose hamare paas ek original file hai:

```text
original-file
```

Ab hum ek Symbolic Link create karna chahte hain jo sirf original file ka path point kare.

---

# Lab Environment

Apni working directory mein jayein.

Example:

```bash
cd /root/links
```

Current location verify karein:

```bash
pwd
```

Example Output:

```text
/root/links
```

Shuru mein directory khaali hogi.

---

# 🔬 Lab 1 – Original File Create Karein

Original file create karein:

```bash
echo "This is my soft link test file." > original-file
```

File ko verify karein:

```bash
cat original-file
```

Expected Output:

```text
This is my soft link test file.
```

---

# Original File Verify Karein

File ki details dekhein:

```bash
ls -li
```

Example Output:

```text
1234567 original-file
```

Har file ka Linux mein apna unique inode number hota hai.

---

# 🔬 Lab 2 – Soft Link Create Karein

Soft Link banane ke liye command hai:

```bash
ln -s
```

General Syntax:

```bash
ln -s <original_file> <soft_link_name>
```

Example:

```bash
ln -s original-file softlink1
```

Yahan:

- `original-file` → Original File
- `softlink1` → Symbolic Link

---

# Soft Link Verify Karein

Run karein:

```bash
ls -li
```

Example Output:

```text
1234567 original-file

7654321 softlink1 -> original-file
```

Yahan do important cheezein notice karein:

1. Soft Link ka inode number alag hai.
2. File ke permissions ki shuruat:

```text
l
```

se ho rahi hai.

---

# Directory Listing Ko Samjhein

Example:

```text
lrwxrwxrwx
```

Yahan pehla character:

```text
l
```

ka matlab hai:

```text
Link
```

Yeh Linux ko batata hai ke yeh ek Symbolic Link hai.

---

# Soft Link Illustration

```text
Original File
      │
      ▼
original-file
      ▲
      │
softlink1
```

Soft Link sirf original file ko point karta hai.

---

# 🔬 Lab 3 – Soft Link Read Karein

Soft Link ko display karein:

```bash
cat softlink1
```

Expected Output:

```text
This is my soft link test file.
```

Aap Soft Link read kar rahe hain lekin Linux automatically original file ko open kar raha hai.

---

# 🔬 Lab 4 – Original File Delete Karein

Original file delete karein:

```bash
rm original-file
```

Ab Soft Link ko access karein:

```bash
cat softlink1
```

Expected Output:

```text
No such file or directory
```

Soft Link ab bhi maujood hai lekin jis file ko point kar raha tha woh delete ho chuki hai.

Is liye yeh ab **Broken Link** ban gaya hai.

---

# Broken Link Illustration

```text
Original File
      ❌ Deleted

Soft Link
      │
      ▼
Broken Link
```

---

# Directory Listing Delete Hone Ke Baad

Run karein:

```bash
ls -l
```

Bohat si Linux distributions Broken Links ko **red color** mein dikhati hain.

Yeh indicate karta hai ke target file ab exist nahi karti.

---

# Broken Link Delete Karein

Broken Soft Link remove karein:

```bash
rm softlink1
```

---

# 🔬 Lab 5 – Nayi Original File Create Karein

Ek nayi file create karein:

```bash
echo "This is my second original file." > original-file2
```

Verify karein:

```bash
cat original-file2
```

---

# 🔬 Lab 6 – Different Directory Mein Soft Link Create Karein

Suppose aap Soft Link banana chahte hain:

```text
/tmp
```

directory ke andar.

Command:

```bash
ln -s /root/links/original-file2 /tmp/softlink2
```

Yahan hum ne **Absolute Path** use kiya hai.

---

# Link Verify Karein

Run karein:

```bash
ls -l /tmp
```

Output kuch is tarah hogi:

```text
softlink2 -> /root/links/original-file2
```

Ab verify karein:

```bash
cat /tmp/softlink2
```

Expected Output:

```text
This is my second original file.
```

Soft Link bilkul sahi kaam karega.

---

# Absolute Path Kyun Zaroori Hai?

Jab aap kisi doosri directory mein Soft Link create karte hain to **Absolute Path** dena best practice hota hai.

Example:

```bash
ln -s /root/links/original-file2 /tmp/softlink2
```

Is tarah Linux hamesha original file tak sahi pohanch jata hai.

---

# 🔬 Lab 7 – Relative Path Ka Issue

Suppose aap Soft Link is tarah create karein:

```bash
ln -s original-file2 /tmp/softlink3
```

Yeh Relative Path hai.

Ab run karein:

```bash
cat /tmp/softlink3
```

Expected Output:

```text
No such file or directory
```

Kyun?

Kyun ke Soft Link ab:

```text
/tmp/original-file2
```

ko dhoond raha hai.

Jab ke actual file hai:

```text
/root/links/original-file2
```

Is liye Relative Path galat location ko point karta hai.

---

# Relative Path vs Absolute Path

### Relative Path

```text
original-file2
```

Current location par depend karta hai.

---

### Absolute Path

```text
/root/links/original-file2
```

Hamesha correct file ko point karta hai.

Is liye jab bhi different directory mein Soft Link create karein to **Absolute Path** use karein.

---

# 🔬 Lab 8 – Different File System Mein Soft Link Create Karein

Hard Link ke baraks Soft Link different file systems ke across create ho sakta hai.

Suppose:

```text
/boot
```

ek alag file system hai.

Run karein:

```bash
ln -s /root/links/original-file2 /boot/softlink4
```

Verify karein:

```bash
cat /boot/softlink4
```

Expected Output:

```text
This is my second original file.
```

Soft Link successfully kaam karega.

---

# Yeh Kyun Kaam Karta Hai?

Soft Link sirf **file ka path** store karta hai.

Yeh inode ko directly reference nahi karta.

Is liye Linux different mounted file systems ke across bhi path follow kar leta hai.

---

# Soft Link vs Hard Link

| Feature | Soft Link | Hard Link |
|----------|-----------|-----------|
| Kis ko point karta hai | File Path | Inode |
| Same inode | No | Yes |
| Same permissions | No | Yes |
| Original delete hone par | Broken Link | Kaam karta rehta hai |
| Different File Systems | Yes | No |
| Directories ko point kar sakta hai | Yes | No |

---

# Practice Exercises

## Exercise 1

Practice directory create karein:

```bash
mkdir ~/softlink-lab
```

---

## Exercise 2

Ek file create karein:

```bash
echo "Linux Soft Link Lab" > original.txt
```

---

## Exercise 3

Soft Link create karein:

```bash
ln -s original.txt shortcut.txt
```

---

## Exercise 4

Inode numbers verify karein:

```bash
ls -li
```

Notice karein ke inode numbers alag hain.

---

## Exercise 5

Soft Link read karein:

```bash
cat shortcut.txt
```

---

## Exercise 6

Original file delete karein:

```bash
rm original.txt
```

Ab Soft Link read karein:

```bash
cat shortcut.txt
```

Error observe karein.

---

## Exercise 7

Absolute Path use karte hue Soft Link create karein:

```bash
ln -s /home/student/file.txt /tmp/file-link
```

Verify karein.

---

## Exercise 8

Ab Relative Path use karke same kaam karein aur difference observe karein.

---

# 🔧 Troubleshooting

### Problem

Soft Link show kar raha hai:

```text
No such file or directory
```

Check karein:

- Kya original file ab bhi exist karti hai?
- Kya path sahi diya gaya hai?

---

### Problem

Soft Link red color mein show ho raha hai.

Yeh normally **Broken Link** ko indicate karta hai.

Target file missing hai.

---

### Problem

Soft Link galat file ko point kar raha hai.

Verify karein:

```bash
ls -l
```

Confirm karein ke target path sahi hai.

---

# 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `ln -s source destination` | Soft Link create karta hai |
| `ls -li` | Inode numbers show karta hai |
| `ls -l` | Link target show karta hai |
| `cat file` | File ka content display karta hai |
| `rm file` | File ya Link delete karta hai |

---

# 📖 Key Takeaways

- Soft Link original file ka path store karta hai.
- Soft Link ka apna inode number hota hai.
- `l` character Symbolic Link ko identify karta hai.
- Original file delete hone par Soft Link Broken Link ban jata hai.
- Soft Link directories ko bhi point kar sakta hai.
- Soft Link different file systems ke across create ho sakta hai.
- Different directory mein Soft Link create karte waqt hamesha Absolute Path use karein.

---

# 💡 Yaad Rakhein

> **Soft Link ko Google Maps ke address ki tarah samjhein.**
>
> - Google Maps sirf destination ka address store karta hai.
> - Agar building gira di jaye to address phir bhi rehta hai, lekin destination tak nahi pohanch sakte.
> - Isi tarah Soft Link sirf original file ka path store karta hai.
> - Agar original file delete ho jaye to Soft Link **Broken Link** ban jata hai.
>
> **Golden Rule:**
>
> ```text
> Soft Link
>      │
>      ▼
> File Path Ko Point Karta Hai
>
> Original File Delete
>      │
>      ▼
> Broken Link
> ```
>
> Hard Links ke muqablay mein Soft Links zyada flexible hote hain kyun ke yeh different file systems aur directories dono ko point kar sakte hain.