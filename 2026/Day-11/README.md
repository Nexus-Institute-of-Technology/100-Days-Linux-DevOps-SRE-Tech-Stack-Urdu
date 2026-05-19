# Practice Lab – Linux Shell Basics
>Date: 19th May, 2026 - Day 11

---

# Table of Contents

| Task | Title |
|------|--------|
| 1 | [Linux Shell Kya Hai?](#task-1-linux-shell-kya-hai) |
| 2 | [Linux mein Available Shells](#task-2-linux-mein-available-shells) |
| 3 | [Linux Shell Access Karne ke Tareeqay](#task-3---linux-shell-access-karne-ke-tareeqay) |
| 4 | [Bash Shell Explore Karein](#task-4-bash-shell-explore-karein) |
| 5 | [Linux mein date Command Explore Karein](#task-5---linux-mein-date-command-explore-karein) |
| 6 | [Linux mein Help Kaise Lein?](#task-6-linux-mein-help-kaise-lein) |
| 7 | [Final Summary](#final-summary) |

---

# Task 1: Linux Shell Kya Hai?

Shell aapko Unix/Linux based system ke saath interact karne ke liye ek interface provide karta hai. Yeh aapse input gather karta hai aur us input ke basis par programs execute karta hai. Jab program execute ho jata hai to shell uska output display karta hai.

```text
Your Input -> Linux Shell -> Kernel Helps to Execute -> Output Display
```

Shell human readable commands ko accept karta hai aur unhein aisi language mein translate karta hai jo kernel samajh aur process kar sake.

Shell Unix/Linux systems ke saath interact karne ke liye interface provide karta hai. Yeh aapse input leta hai aur uske basis par programs execute karta hai. Jab program finish hota hai to uska output display karta hai.

Shell human readable commands ko translate karta hai taake kernel unhein samajh aur process kar sake.

---

### Question

Upar diye gaye kisi bhi simple sentence ko use karke apne alfaaz mein explain karein ke aapne Linux Shell ke bare mein kya samjha.

> NOTE: Yeh answer aapko apni "Final Summary" mein likhna hai.

---

# Task 2: Linux mein Available Shells

## Common Linux Shells

### 1. BASH (Bourne-Again Shell)

- Linux ka sabse common shell
- Open Source
- Bohat si Linux distributions ka default shell

---

### 2. CSH (C Shell)

- Iska syntax C programming language jaisa hota hai

---

### 3. KSH (Korn Shell)

- David Korn ne AT&T Bell Labs mein create kiya
- POSIX shell standards ki foundation bana

---

### 4. TCSH

- Berkeley UNIX C Shell (CSH) ka enhanced version

---

### Question

Linux System Administrator ke tor par aap sabse zyada kaunsa shell use karenge?

> NOTE: Yeh answer aapko apni "Final Summary" mein likhna hai.

---

# Task 3 - Linux Shell Access Karne ke Tareeqay

### 1. Secure Shell (SSH) ke zariye

Examples:

- PuTTY
- Super PuTTY
- MobaXterm
- Guacamole

---

### 2. CMD Prompt Terminal ke zariye

Aap Windows CMD ya PowerShell ke through Linux systems access kar sakte hain.

---

### 3. Console ke zariye

Example:

- Xen-Orchestra Console

---

# Task 4: Bash Shell Explore Karein

### Run:

```bash
# System mein available shells dekhne ke liye:
cat /etc/shells
```

#### Expected Output:

```bash
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
```

---

#### Apna Current Shell Check Karein

Run:

```bash
echo $SHELL
```

#### Expected Output:

```bash
/bin/bash
```

---

#### Apne Shell ka Process ID (PID) Check Karein

Run:

```bash
ps $$
```

#### Example Output:

```bash
PID TTY      STAT   TIME COMMAND
17217 pts/0  Ss     0:00 -bash
```

> NOTE: Aapka PID number different hoga.

---

# Task 5 - Linux mein date Command Explore Karein

#### Run:

```bash
date
```

#### Example Output:

```bash
Mon May 18 11:36:58 PM CDT 2026
```

---

#### Check Karein ke date Command Kaise Execute Hoti Hai

Run:

```bash
whereis date
```

#### Example Output:

```bash
date: /usr/bin/date /usr/share/man/man1/date.1.gz
```

---

#### Explanation

Yeh output batata hai ke backend mein `/usr/bin/date` program execute ho raha hai jo Kernel ke saath communicate karta hai.

---

# Task 6: Linux mein Help Kaise Lein?

#### Linux Manual Pages Use Karein

Run:

```bash
man date
```

#### Explanation

- Yeh command `date` command ka manual page open karegi
- Manual se bahar nikalne ke liye keyboard par `q` press karein

---

#### Example Output

```text
NAME
       date - print or set the system date and time
```

---

### Linux Bash Shell mein Help Use Karna

#### Run:

```bash
uptime --help
```

#### Example Output:

```bash
Usage:
 uptime [options]

Options:
 -p, --pretty   show uptime in pretty format
 -h, --help     display this help and exit
 -s, --since    system up since
 -V, --version  output version information and exit
```

---

# Final Summary

#### Questions

### 1. Aap ne ab tak kitni commands seekhi hain?

Commands learned so far:

- pwd
- cd
- touch
- mkdir -p
- ls
- ll
- date
- uptime
- clear
- Ctrl + C
- man

---

### 2. Linux Shell ke bare mein aapne kya samjha?

Apne alfaaz mein short explanation likhein.

---

### 3. Linux System Administrators sabse zyada kaunsa shell use karte hain?

Expected Answer:

```text
BASH (Bourne-Again Shell)
```

---

# Important Activity

## Apni Final Summary ko LinkedIn par Post Karein

Share karein:

- Aapne kya seekha
- Kaunsi commands practice ki
- Linux Shell ke bare mein aapki understanding
- Aapka Linux learning journey

---

Source File: :contentReference[oaicite:0]{index=0}