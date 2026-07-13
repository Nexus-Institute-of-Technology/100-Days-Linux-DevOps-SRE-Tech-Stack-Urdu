# MODULE 11 – Practice Lab: `tee` Command in Linux
> **Hands-on Practice Lab – `tee` Command Ka Istemaal (Screen Par Bhi Aur File Mein Bhi Output Save Karna)**

---

# 🎯 Lab Objectives

Is lab ke end tak aap seekh jayenge:

- `tee` command ka purpose kya hai.
- `tee` command pipelines ke sath kaise kaam karti hai.
- Command ka output screen par dikhate hue file mein save karna.
- `tee -a` ki madad se existing file mein output append karna.
- `tee` ko doosri Linux commands aur pipelines ke sath use karna.

---

# 📖 Introduction

Pichle lesson mein hum ne **Pipelines** ke bare mein seekha tha.

Kabhi kabhi hamari requirement hoti hai ke:

- Command ka output terminal par bhi nazar aaye.
- Aur wahi output file mein bhi save ho jaye.

Normally agar hum output redirection (`>`) use karte hain to output file mein chala jata hai, lekin screen par display nahi hota.

Is problem ka solution **`tee` command** hai.

`tee` command ek hi waqt mein:

- Output ko screen par display karti hai.
- Aur usi output ko file mein bhi save karti hai.

---

# `tee` Command Kya Hai?

`tee` command **Standard Input (stdin)** se data receive karti hai aur ek hi waqt mein do kaam karti hai:

1. Output ko terminal par display karti hai.
2. Usi output ko ek ya zyada files mein save karti hai.

General Syntax:

```bash
command | tee filename
```

---

# `tee` Command Kaise Kaam Karti Hai?

```text
Command
    │
    ▼
 Standard Output
    │
    ▼
   Pipe (|)
    │
    ▼
     tee
   ┌───────┐
   │       │
   ▼       ▼
Screen   File
```

Output ki do copies ban jati hain:

- Ek Screen par display hoti hai.
- Doosri File mein save hoti hai.

---

# `tee` Kyun Use Karte Hain?

Agar hum sirf output redirection use karein:

```bash
command > output.txt
```

Result:

- Output file mein save ho jayega.
- Screen par kuch bhi display nahi hoga.

---

Agar `tee` use karein:

```bash
command | tee output.txt
```

Result:

- Output screen par bhi nazar aayega.
- Aur file mein bhi save ho jayega.

Yahi `tee` command ka sab se bara advantage hai.

---

# Example 1 – Output Redirect Karna Without `tee`

Suppose aap directory listing ki sirf pehli paanch lines save karna chahte hain.

Command:

```bash
ls -ltr | head -n 5 > files.txt
```

Result:

- Output `files.txt` mein save ho jayega.
- Screen par kuch bhi display nahi hoga.

Verify karein:

```bash
cat files.txt
```

---

# Example 2 – `tee` Ke Sath Output Display Aur Save Karna

Ab wahi kaam `tee` ke sath karein:

```bash
ls -ltr | head -n 5 | tee files.txt
```

Result:

- Pehli paanch lines screen par display hongi.
- Wahi output `files.txt` mein bhi save ho jayega.

Yahi `tee` command ka main benefit hai.

---

# Pipeline Flow

```text
ls -ltr
     │
     ▼
head -n 5
     │
     ▼
tee files.txt
   ┌───────┐
   │       │
   ▼       ▼
Screen   files.txt
```

---

# File Verify Karein

Saved file ko display karein:

```bash
cat files.txt
```

Jo output screen par nazar aaya tha wahi file ke andar bhi hoga.

---

# Example 3 – Current Date Save Karna

Suppose aap command chalate hain:

```bash
date
```

Normally date sirf screen par display hoti hai.

Agar usay screen par bhi dekhna hai aur file mein bhi save karna hai:

```bash
date | tee date.txt
```

Example Output:

```text
Tue Jul 14 10:25:30 AM UTC 2026
```

Output:

- Screen par bhi nazar aayega.
- Aur `date.txt` ke andar bhi save ho jayega.

Verify karein:

```bash
cat date.txt
```

---

# Example 4 – Output Append Karna

Default taur par `tee` file ko overwrite karta hai.

Example:

```bash
date | tee date.txt
```

Har dafa purana content replace ho jayega.

Agar purana data delete nahi karna aur naya data uske end mein add karna ho to:

```bash
command | tee -a filename
```

Example:

```bash
date | tee -a date.txt
```

Ab har execution ke baad nayi line file ke end mein add hoti rahegi.

---

# Append Flow

```text
Command
    │
    ▼
   tee -a
    │
    ▼
Existing File
    │
    ▼
New Output Add Ho Jata Hai
```

Purana data delete nahi hota.

---

# Append Verify Karein

Run karein:

```bash
cat date.txt
```

Example Output:

```text
Tue Jul 14 10:20:00 AM UTC 2026
Tue Jul 14 10:25:30 AM UTC 2026
Tue Jul 14 10:30:15 AM UTC 2026
```

Har dafa nayi line file ke end mein add hoti jayegi.

---

# `tee` vs Output Redirection

| Method | Screen Par Display | File Mein Save |
|----------|-------------------|----------------|
| `>` | No | Yes |
| `>>` | No | Yes (Append) |
| `tee` | Yes | Yes |
| `tee -a` | Yes | Yes (Append) |

---

# Common Examples

Directory listing display aur save karein:

```bash
ls -ltr | tee files.txt
```

---

Sirf first five lines display aur save karein:

```bash
ls -ltr | head -n 5 | tee top5.txt
```

---

Current date display aur save karein:

```bash
date | tee date.txt
```

---

Date append karein:

```bash
date | tee -a date.txt
```

---

Running processes display aur save karein:

```bash
ps -ef | tee processes.txt
```

---

SSH processes search karein aur save karein:

```bash
ps -ef | grep ssh | tee ssh-processes.txt
```

---

# Practice Exercises

## Exercise 1

Current date display aur save karein:

```bash
date | tee today.txt
```

---

## Exercise 2

Current date append karein:

```bash
date | tee -a today.txt
```

---

## Exercise 3

First five files save karein:

```bash
ls -ltr | head -n 5 | tee files.txt
```

---

## Exercise 4

Running processes save karein:

```bash
ps -ef | tee processes.txt
```

---

## Exercise 5

SSH processes search karke save karein:

```bash
ps -ef | grep ssh | tee ssh.txt
```

---

# 🔧 Troubleshooting

### Problem

Output screen par display nahi ho raha.

Check karein ke aap:

```bash
tee
```

use kar rahe hain, na ke:

```bash
>
```

---

### Problem

Har dafa file overwrite ho rahi hai.

Use karein:

```bash
tee -a
```

instead of:

```bash
tee
```

---

### Problem

File empty hai.

Confirm karein ke pehli command output generate kar rahi hai.

Example:

```bash
ls
```

---

# 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `tee file.txt` | Output screen par bhi dikhata hai aur file mein bhi save karta hai |
| `tee -a file.txt` | Output screen par dikhata hai aur file mein append karta hai |
| `>` | Output file mein redirect karta hai |
| `>>` | Output file mein append karta hai |
| `cat file.txt` | File ka content display karta hai |

---

# 📖 Key Takeaways

- `tee` command **stdin** se input receive karti hai.
- Yeh output ko screen par bhi display karti hai aur file mein bhi save karti hai.
- `tee` ko zyada tar pipelines ke sath use kiya jata hai.
- `tee -a` output ko overwrite nahi karta balke append karta hai.
- `tee` command logging aur monitoring dono ke liye bohot useful hai.

---

# 💡 Yaad Rakhein

> **`tee` command ko road ke junction ki tarah samjhein.**
>
> - Ek road se traffic aata hai (Command Output).
> - Junction par traffic do directions mein divide ho jata hai.
> - Ek road Screen ki taraf jata hai.
> - Doosra road File ki taraf jata hai.
> - Dono jagah ek hi information ek hi waqt mein pohanchti hai.
>
> **Golden Rule:**
>
> ```text
> Command
>     │
>     ▼
>   Pipe (|)
>     │
>     ▼
>    tee
>   ┌───────┐
>   │       │
>   ▼       ▼
> Screen   File
> ```
>
> **`tee` command ki madad se aap command ka output live bhi dekh sakte hain aur usay future reference ke liye file mein bhi save kar sakte hain.**