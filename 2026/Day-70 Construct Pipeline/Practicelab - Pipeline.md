# MODULE 11 – Construct Pipelines
> **Linux Mein Pipelines Banana aur Istemaal Karna**

---

# 🎯 Learning Objectives

Is lesson ke end tak aap seekh jayenge:

- Linux mein Pipeline kya hoti hai.
- Pipe (`|`) operator kaise kaam karta hai.
- Standard Output (**stdout**) aur Standard Input (**stdin**) ko samajhna.
- Multiple Linux commands ko Pipeline ke zariye combine karna.
- Command output ko efficiently filter aur process karna.
- Pipeline ke output ko file mein redirect karna.

---

# 📖 Introduction

Linux Command Line ki sab se powerful features mein se ek **Pipeline** hai.

Pipeline aap ko do ya us se zyada commands ko aapas mein connect karne ki ijazat deti hai.

Ek command ka output doosri command ka input ban jata hai.

Is tarah alag alag commands ko manually chalane ke bajaye Linux unhein **Pipe Operator (`|`)** ke zariye directly connect kar deta hai.

Is se command execution zyada fast, clean aur efficient ho jati hai.

---

# Pipeline Kya Hoti Hai?

Pipeline ek sequence hoti hai jisme do ya us se zyada commands ko **Vertical Bar (`|`)**, yani **Pipe** ke zariye separate kiya jata hai.

General Syntax:

```bash
command1 | command2 | command3
```

Pehli command ka output doosri command ka input ban jata hai.

Doosri command ka output teesri command ka input ban jata hai.

Yeh process aakhir tak chalta rehta hai.

---

# Pipeline Kaise Kaam Karti Hai?

```text
Command 1
     │
     ▼
 Standard Output (stdout)
     │
     ▼
     Pipe (|)
     │
     ▼
 Standard Input (stdin)
     │
     ▼
Command 2
```

Output screen par dikhane ke bajaye Linux usay seedha agli command ko de deta hai.

---

# stdout aur stdin Ko Samjhein

Har Linux command ke teen standard data streams hote hain.

| Stream | Description |
|---------|-------------|
| Standard Input (stdin) | Command ka input receive karta hai |
| Standard Output (stdout) | Normal output display karta hai |
| Standard Error (stderr) | Error messages display karta hai |

Pipeline mein:

- Pehli command ka **stdout**, doosri command ka **stdin** ban jata hai.

---

# Pipelines Kyun Use Karte Hain?

Pipeline ki madad se aap:

- Multiple commands combine kar sakte hain.
- Large output ko filter kar sakte hain.
- Search kar sakte hain.
- Count kar sakte hain.
- Output ko page-wise dekh sakte hain.
- Output ko file mein save kar sakte hain.

Yeh sab ek hi command line mein ho jata hai.

---

# Example 1 – Files Count Karna

Suppose aap command chalate hain:

```bash
ls
```

Yeh current directory ke tamam files aur directories show karegi.

Agar sirf total entries count karni hon:

```bash
ls | wc -l
```

---

# Yeh Kaise Kaam Karta Hai?

```text
ls
 │
 ▼
Files Ki List
 │
 ▼
Pipe (|)
 │
 ▼
wc -l
 │
 ▼
Lines Ki Count
```

`wc -l` ko `ls` ka output milta hai aur woh total lines count kar deta hai.

Example Output:

```text
15
```

---

# Example 2 – Long Output Ko Page Wise Dekhna

Suppose aap chalate hain:

```bash
ls -l /usr/bin
```

Is command ka output bohot lamba hota hai.

Screen se pehle wali lines scroll ho jati hain.

Is problem ka solution:

```bash
ls -l /usr/bin | less
```

Ab output **less** program ke andar open hogi.

Aap:

- **Enter** → Ek line neeche
- **Space** → Ek page neeche
- **b** → Ek page upar
- **q** → Quit

---

# Pipeline Flow

```text
ls -l /usr/bin
        │
        ▼
Standard Output
        │
        ▼
       Pipe
        │
        ▼
      less
```

`ls` ka output `less` ka input ban gaya.

---

# Example 3 – Sirf First Five Lines Dekhna

Suppose aap command chalate hain:

```bash
ls -ltr
```

Lekin aap sirf pehli paanch lines dekhna chahte hain.

Command:

```bash
ls -ltr | head -n 5
```

---

# Yeh Kaise Kaam Karta Hai?

```text
ls -ltr
     │
     ▼
Directory Listing
     │
     ▼
Pipe
     │
     ▼
head -n 5
     │
     ▼
First Five Lines
```

Example Output:

```text
total 20
-rw-r--r-- file1
-rw-r--r-- file2
drwxr-xr-x dir1
drwxr-xr-x dir2
```

---

# Example 4 – Pipeline Output Ko File Mein Save Karna

Pipeline ka final output file mein bhi save kiya ja sakta hai.

Example:

```bash
ls -ltr | head -n 5 > output.txt
```

Ab pehli paanch lines file:

```text
output.txt
```

mein save ho jayengi.

Verify karein:

```bash
cat output.txt
```

Example Output:

```text
total 20
-rw-r--r-- file1
-rw-r--r-- file2
drwxr-xr-x dir1
drwxr-xr-x dir2
```

---

# Pipeline + Output Redirection

```text
ls -ltr
     │
     ▼
Pipe
     │
     ▼
head -n 5
     │
     ▼
Output Redirection (>)
     │
     ▼
output.txt
```

Yahan Pipeline aur Output Redirection dono ek sath use hue hain.

---

# Pipelines Ke Advantages

Pipeline ke bohot se faide hain:

- Manual work kam hota hai.
- Temporary files ki zarurat nahi hoti.
- Commands efficient ho jati hain.
- Multiple commands combine ho jati hain.
- Data processing asaan ho jati hai.
- Powerful command chaining possible hoti hai.

---

# Common Pipeline Commands

| Command | Purpose |
|----------|---------|
| `ls` | Files list karta hai |
| `cat` | File ka content display karta hai |
| `grep` | Text search karta hai |
| `head` | Pehli lines display karta hai |
| `tail` | Aakhri lines display karta hai |
| `sort` | Output sort karta hai |
| `uniq` | Duplicate lines remove karta hai |
| `wc` | Lines, words aur characters count karta hai |
| `less` | Output page-wise display karta hai |

---

# Example Pipeline Commands

Files count karein:

```bash
ls | wc -l
```

SSH process search karein:

```bash
ps -ef | grep ssh
```

Pehle users display karein:

```bash
cat /etc/passwd | head
```

Last five log entries dekhein:

```bash
cat /var/log/messages | tail -n 5
```

Usernames sort karein:

```bash
cat users.txt | sort
```

---

# Practice Exercises

## Exercise 1

Files list karein aur count karein:

```bash
ls | wc -l
```

---

## Exercise 2

`/usr/bin` ko page-wise display karein:

```bash
ls -l /usr/bin | less
```

---

## Exercise 3

Pehli paanch files display karein:

```bash
ls -ltr | head -n 5
```

---

## Exercise 4

Aakhri paanch files display karein:

```bash
ls -ltr | tail -n 5
```

---

## Exercise 5

Pehli paanch lines file mein save karein:

```bash
ls -ltr | head -n 5 > files.txt
```

Verify karein:

```bash
cat files.txt
```

---

# 🔧 Troubleshooting

### Problem

Output show nahi ho raha.

Check karein ke pehli command output produce kar rahi hai ya nahi.

Example:

```bash
ls
```

---

### Problem

Doosri command expected result nahi de rahi.

Check karein ke woh **stdin** se input receive karti hai ya nahi.

---

### Problem

Output bohot lamba hai.

Use karein:

```bash
| less
```

taake output page-wise dekh sakein.

---

# 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `|` | Commands ko connect karta hai |
| `ls \| wc -l` | Directory entries count karta hai |
| `ls -l /usr/bin \| less` | Output page-wise dikhata hai |
| `head -n 5` | Pehli paanch lines display karta hai |
| `tail -n 5` | Aakhri paanch lines display karta hai |
| `>` | Output ko file mein redirect karta hai |

---

# 📖 Key Takeaways

- Pipeline do ya us se zyada Linux commands ko connect karti hai.
- Pipe operator `|` se represent hota hai.
- Ek command ka **stdout**, agli command ka **stdin** ban jata hai.
- Pipelines commands ko mil kar efficiently kaam karne deti hain.
- Pipelines manual work ko kam karti hain.
- Pipelines ko Output Redirection ke sath combine kiya ja sakta hai.

---

# 💡 Yaad Rakhein

> **Pipeline ko factory ki assembly line ki tarah samjhein.**
>
> - Pehla worker apna kaam complete karta hai.
> - Woh result foran doosre worker ko de deta hai.
> - Doosra worker us par apna kaam karta hai aur teesre ko pass kar deta hai.
> - Isi tarah har Linux command apna kaam karti hai aur Pipe (`|`) result ko agli command tak pohancha deta hai.
>
> **Golden Rule:**
>
> ```text
> Command 1
>      │
>      ▼
> Standard Output
>      │
>      ▼
> Pipe (|)
>      │
>      ▼
> Standard Input
>      │
>      ▼
> Command 2
> ```
>
> Pipelines Linux ki sab se powerful features mein se ek hain jo multiple commands ko efficiently ek sath kaam karne ki capability deti hain.