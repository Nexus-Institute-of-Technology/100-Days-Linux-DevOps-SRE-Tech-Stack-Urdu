# MODULE 08 – Practice Lab: Linux Processes Ki Listing
> **Hands-on Practice Lab – `ps` Command Se Linux Processes Dekhna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `ps` command se Linux processes display karna.
- `ps` aur `top` ke darmiyan farq samajhna.
- Current terminal se related processes dekhna.
- System par chalne wale tamam processes dekhna.
- Detailed process information dekhna.
- Lambi output ko `less` ke saath page by page read karna.
- `ps` command ke long-listing options use karna.

---

# 📖 Introduction

Linux system troubleshoot karte waqt yeh samajhna bohot zaroori hai:

- Kernel processes ko kis tarah manage karta hai.
- Processes Kernel ke saath kis tarah communicate karte hain.
- Processes aapas mein kis tarah communicate karte hain.
- Is waqt kaun se processes run kar rahe hain.
- Process ka owner kaun hai.
- Process kitna CPU aur memory use kar raha hai.
- Process kis state mein hai.

Linux process information dekhne ke liye mukhtalif commands provide karta hai.

Sab se zyada use hone wali do commands hain:

```bash
ps
```

aur:

```bash
top
```

---

# 1. `ps` Command Kya Hai?

`ps` ka matlab hai:

> **Process Status**

Ye command currently running processes ki information display karti hai.

`top` ke baraks, `ps` command us waqt ke processes ka sirf ek **single snapshot** dikhati hai jab command execute hoti hai.

---

# Basic Syntax

```bash
ps
```

Default tor par ye command current terminal session se associated processes dikhati hai.

---

# 🔬 Lab 1 – Current Terminal Ke Processes Display Karein

Command chalayein:

```bash
ps
```

Example Output:

```text
PID TTY          TIME CMD
2180 pts/0    00:00:00 bash
2251 pts/0    00:00:00 ps
```

---

# Default `ps` Output Ko Samjhein

| Column | Matlab |
|--------|--------|
| `PID` | Process ID |
| `TTY` | Process ke saath associated terminal |
| `TIME` | Process ke zariye use kiya gaya total CPU time |
| `CMD` | Command ya process ka naam |

---

# Default `ps` Command Kya Dikhati Hai?

Default `ps` command aam tor par dikhati hai:

- Current shell.
- Current terminal mein chalne wali commands.
- `ps` command khud.

Ye system ke tamam processes display nahi karti.

---

# 2. `ps aux` Se Tamam Processes Dekhein

System ke lagbhag tamam processes dekhne ke liye:

```bash
ps aux
```

Ye BSD-style options use karta hai aur in options ke saath hyphen lagana zaroori nahi hota.

---

# `aux` Ka Matlab

| Option | Matlab |
|--------|--------|
| `a` | Tamam users ke terminal-attached processes show kare |
| `u` | User-oriented details show kare |
| `x` | Bina controlling terminal wale processes bhi include kare |

In teenon ko mila kar:

```text
aux
```

system ke lagbhag tamam running processes dikhata hai.

---

# 🔬 Lab 2 – System Ke Tamam Processes Display Karein

Command:

```bash
ps aux
```

Typical header:

```text
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
```

---

# `ps aux` Ke Process Columns

| Column | Matlab |
|--------|--------|
| `USER` | Process ka owner |
| `PID` | Process ID |
| `%CPU` | CPU usage percentage |
| `%MEM` | Physical memory usage percentage |
| `VSZ` | Virtual memory size |
| `RSS` | Resident physical memory |
| `TTY` | Controlling terminal |
| `STAT` | Process state |
| `START` | Process start hone ka time |
| `TIME` | Accumulated CPU time |
| `COMMAND` | Complete command line |

---

# 3. Lambi Output Ke Liye `less` Use Karein

`ps aux` ki output bohot lambi ho sakti hai.

Isay page by page read karne ke liye pipe aur `less` use karein.

```bash
ps aux | less
```

---

# Pipe Ko Samjhein

Pipe ka symbol hai:

```text
|
```

Ye ek command ki output doosri command ko bhejta hai.

Example:

```bash
ps aux | less
```

Flow:

```text
ps aux Ki Output
       │
       ▼
     less
       │
       ▼
Page-by-Page Display
```

---

# Useful `less` Keys

| Key | Kaam |
|-----|------|
| `Space` | Ek page aage jaye |
| `b` | Ek page peeche jaye |
| `/text` | Text search kare |
| `n` | Agla search result dikhaye |
| `q` | Exit kare |

---

# 🔬 Lab 3 – Process Output Page by Page Dekhein

Run karein:

```bash
ps aux | less
```

Practice:

- `Space` press karke aage jayein.
- `b` press karke peeche jayein.
- `/sshd` type karke SSH processes search karein.
- `q` press karke exit karein.

---

# 4. `ps` Aur `top` Mein Farq

Dono commands process information dikhati hain, lekin in ka kaam mukhtalif hai.

| Feature | `ps` | `top` |
|---------|------|-------|
| Output Type | Single snapshot | Dynamic real-time display |
| Automatically Refresh | Nahi | Haan |
| Process Details | Haan | Haan |
| System Summary | Limited | Detailed |
| Best Use | Scripts, filtering, reports | Live troubleshooting |
| Pipe Ke Saath Use | Bohot aasaan | Kam use hota hai |

---

# Example Comparison

## `ps`

```bash
ps aux
```

Output ek dafa display hoti hai aur command exit ho jati hai.

---

## `top`

```bash
top
```

Output continuously refresh hoti rehti hai jab tak aap quit na karein.

---

# 5. `ps -l` Se Long Listing

Current terminal ke processes ki long-format listing ke liye:

```bash
ps -l
```

Typical header:

```text
F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD
```

---

# Important Long-Listing Columns

| Column | Matlab |
|--------|--------|
| `F` | Process flags |
| `S` | Process state |
| `UID` | User ID |
| `PID` | Process ID |
| `PPID` | Parent Process ID |
| `C` | Processor utilization value |
| `PRI` | Kernel scheduling priority |
| `NI` | Nice value |
| `SZ` | Process memory size |
| `WCHAN` | Kernel function jahan process wait kar raha hai |
| `TTY` | Controlling terminal |
| `TIME` | Accumulated CPU time |
| `CMD` | Command name |

---

# 6. `ps lax` Se Detailed Long Listing

System ke bohot se processes ki long-format listing ke liye:

```bash
ps lax
```

Ye BSD-style options use karta hai.

---

# `lax` Ka Matlab

| Option | Matlab |
|--------|--------|
| `l` | Long format |
| `a` | Doosre users ke processes bhi include kare |
| `x` | Bina controlling terminal wale processes include kare |

---

# 🔬 Lab 4 – Long Process Listing Display Karein

Run karein:

```bash
ps lax
```

Page by page output ke liye:

```bash
ps lax | less
```

Ye detailed information dikhata hai:

- Process state
- PID
- PPID
- Priority
- Nice value
- Memory information
- Waiting channel
- Command

---

# 7. `ps` Mein Process States Samjhein

`STAT` ya `S` column process ki state dikhata hai.

| State | Matlab |
|-------|--------|
| `R` | Running ya Runnable |
| `S` | Interruptible Sleep |
| `D` | Uninterruptible Sleep |
| `T` | Stopped |
| `Z` | Zombie |
| `I` | Idle Kernel Thread |

---

# Example

```text
USER       PID %CPU %MEM STAT COMMAND
root         1  0.0  0.1 Ss   /usr/lib/systemd/systemd
root       820  0.0  0.0 S    /usr/sbin/sshd
dev1      1300  0.1  0.1 R+   ps aux
```

`STAT` ka pehla character main process state dikhata hai.

Extra characters mazeed information provide karte hain.

---

# 8. Common Additional `STAT` Characters

| Character | Matlab |
|-----------|--------|
| `s` | Session leader |
| `+` | Foreground process group |
| `<` | High-priority process |
| `N` | Low-priority process |
| `l` | Multithreaded process |

Example:

```text
Ss
```

Matlab:

- `S` = Sleeping
- `s` = Session Leader

---

# 9. Useful `ps` Commands

| Command | Kaam |
|---------|------|
| `ps` | Current terminal ke processes |
| `ps aux` | System ke lagbhag tamam processes |
| `ps aux \| less` | Tamam processes page by page |
| `ps -l` | Current terminal ki long listing |
| `ps lax` | Detailed system process listing |
| `ps lax \| less` | Long listing page by page |
| `ps -ef` | Full-format process listing |
| `ps -eo pid,ppid,stat,cmd` | Selected process columns |

---

# 10. `ps aux` Aur `ps -ef` Mein Farq

Dono commands lagbhag tamam processes display karti hain.

```bash
ps aux
```

BSD-style output use karta hai.

```bash
ps -ef
```

UNIX/System V-style output use karta hai.

---

# `ps -ef` Columns

Typical header:

```text
UID PID PPID C STIME TTY TIME CMD
```

| Column | Matlab |
|--------|--------|
| `UID` | Process owner |
| `PID` | Process ID |
| `PPID` | Parent Process ID |
| `C` | CPU utilization indicator |
| `STIME` | Process start time |
| `TTY` | Controlling terminal |
| `TIME` | CPU time used |
| `CMD` | Complete command line |

---

# 11. Selected Process Fields Display Karein

`-o` option se aap apni marzi ke columns select kar sakte hain.

Example:

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,cmd
```

Ye display karega:

- PID
- PPID
- User
- Process State
- CPU Usage
- Memory Usage
- Command

---

# 🔬 Lab 5 – Selected Columns Display Karein

Run karein:

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,cmd | less
```

Jab aap ko sirf specific process information chahiye ho to ye command bohot useful hai.

---

# 12. Specific Process Search Karein

Process output mein search karne ke liye `grep` use karein.

Example:

```bash
ps aux | grep sshd
```

Ye un lines ko display karega jin mein:

```text
sshd
```

mojood ho.

---

# `grep` Command Khud Match Hone Se Bachayein

Behtar command:

```bash
ps aux | grep '[s]shd'
```

Ya:

```bash
pgrep -a sshd
```

---

# 🔬 Lab 6 – SSH Processes Search Karein

Run karein:

```bash
ps aux | grep '[s]shd'
```

Ya:

```bash
pgrep -a sshd
```

---

# 13. Processes Ko CPU Usage Ke Mutabiq Sort Karein

Command:

```bash
ps aux --sort=-%cpu | head
```

Ye sab se zyada CPU use karne wale processes dikhata hai.

---

# 14. Processes Ko Memory Usage Ke Mutabiq Sort Karein

Command:

```bash
ps aux --sort=-%mem | head
```

Ye sab se zyada memory use karne wale processes dikhata hai.

---

# 🔬 Lab 7 – Resource-Intensive Processes Identify Karein

Top CPU consumers:

```bash
ps aux --sort=-%cpu | head
```

Top memory consumers:

```bash
ps aux --sort=-%mem | head
```

---

# 15. Parent Aur Child Relationships Dekhein

Run karein:

```bash
ps -ef --forest
```

Ya:

```bash
ps auxf
```

Ye processes ko tree-like structure mein display karta hai.

---

# Example Process Tree

```text
systemd
 ├─sshd
 │   └─sshd
 │       └─bash
 │           └─ps
 └─crond
```

Is se pata chalta hai ke kaunsa process kis process se create hua.

---

# 🧪 Practice Exercises

## Exercise 1

Current terminal ke processes display karein.

```bash
ps
```

---

## Exercise 2

System ke tamam processes display karein.

```bash
ps aux
```

---

## Exercise 3

Output page by page dekhein.

```bash
ps aux | less
```

---

## Exercise 4

Long listing display karein.

```bash
ps lax | less
```

---

## Exercise 5

Full-format output ke saath tamam processes dekhein.

```bash
ps -ef | less
```

---

## Exercise 6

Selected columns display karein.

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,cmd | less
```

---

## Exercise 7

SSH service ka process dhoondein.

```bash
pgrep -a sshd
```

---

## Exercise 8

Sab se zyada CPU use karne wale processes dekhein.

```bash
ps aux --sort=-%cpu | head
```

---

## Exercise 9

Sab se zyada memory use karne wale processes dekhein.

```bash
ps aux --sort=-%mem | head
```

---

## Exercise 10

Process hierarchy display karein.

```bash
ps -ef --forest | less
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – Running Service Dhoondhna

SSH daemon search karein:

```bash
ps aux | grep '[s]shd'
```

Ya:

```bash
pgrep -a sshd
```

---

### Scenario 2 – High CPU Usage Identify Karna

Run karein:

```bash
ps aux --sort=-%cpu | head
```

---

### Scenario 3 – High Memory Usage Identify Karna

Run karein:

```bash
ps aux --sort=-%mem | head
```

---

### Scenario 4 – Zombie Processes Dhoondhna

Run karein:

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /^Z/'
```

---

### Scenario 5 – Parent Aur Child Processes Samajhna

Run karein:

```bash
ps -ef --forest | less
```

---

# 📌 Quick Revision

| Command | Kaam |
|---------|------|
| `ps` | Current terminal ke processes |
| `ps aux` | Lagbhag tamam running processes |
| `ps aux \| less` | Process output page by page |
| `ps -l` | Current terminal ki long listing |
| `ps lax` | Detailed system process listing |
| `ps -ef` | Full-format process listing |
| `ps -eo ...` | Custom process columns |
| `pgrep -a name` | Process ko naam se dhoonde |
| `ps aux --sort=-%cpu \| head` | Top CPU processes |
| `ps aux --sort=-%mem \| head` | Top memory processes |
| `ps -ef --forest` | Parent-child process tree |

---

# 📖 Key Takeaways

- `ps` running processes ka snapshot display karti hai.
- Default `ps` sirf current terminal se related processes dikhati hai.
- `ps aux` system ke lagbhag tamam processes display karti hai.
- `ps lax` detailed long listing provide karti hai.
- `top` dynamic real-time information deta hai, jab ke `ps` one-time snapshot deta hai.
- Pipe aur `less` lambi output ko read karna aasaan banate hain.
- `grep`, `pgrep`, sorting aur custom fields `ps` ko troubleshooting ke liye bohot useful banate hain.

---

# 💡 Yaad Rakhein

> **`ps` ko Linux processes ki tasveer samjhein.**
>
> - `ps` ek snapshot leta hai.
> - `top` live video provide karta hai.
> - `ps aux` system ke lagbhag tamam processes ki wide picture dikhata hai.
> - `ps lax` mazeed technical details add karta hai.
> - `less` output ko page by page dekhne mein madad karta hai.
>
> **Golden Rule:**
>
> ```text
> Current Terminal Processes = ps
>
> Lagbhag Tamam Processes    = ps aux
>
> Detailed Long Listing      = ps lax
>
> Live Process Monitoring    = top
> ```