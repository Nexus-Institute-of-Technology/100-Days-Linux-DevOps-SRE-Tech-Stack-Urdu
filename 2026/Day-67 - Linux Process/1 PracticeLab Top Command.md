# MODULE 08 – Practice Lab: Linux Process Commands
> **Hands-on Practice Lab – `top` Command Se Linux Processes Monitor Karna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `top` command ka istemal karke Linux processes monitor karna.
- Real-time system dashboard ko samajhna.
- System uptime aur Load Average ko read karna.
- Process States ko samajhna.
- CPU utilization ko analyze karna.
- Physical Memory aur Swap usage ko check karna.
- Process List ke important columns ko samajhna.
- Linux troubleshooting ke dauran `top` command ka istemal karna.

---

# 📖 Introduction

Har Linux System Administrator ke liye zaroori hai ke woh running processes ko monitor karna aur system performance ke issues identify karna seekhe.

Is maqsad ke liye sab se important commands mein se aik command hai:

```bash
top
```

`top` command Linux system ka **dynamic aur real-time view** provide karti hai.

Yeh display karti hai:

- System uptime
- Logged-in users
- Load Average
- Process States
- CPU Utilization
- Memory Usage
- Swap Usage
- Running Processes

Is ka output aam tor par har kuch seconds baad automatically refresh hota rehta hai.

---

# 🔬 Lab 1 – `top` Command Start Karein

Command run karein:

```bash
top
```

Display automatically update hoti rehti hai.

By default yeh har **3 seconds** baad refresh hoti hai.

`top` se bahar nikalne ke liye:

```text
q
```

press karein.

---

# 2. `top` Display Ke Main Areas

Default `top` output do major sections par mushtamil hota hai:

1. **Summary Area**
2. **Process List**

---

## Summary Area

Summary Area screen ke upper hissay mein hota hai.

Yeh information display karta hai:

- Current Time
- System Uptime
- Logged-in Users
- Load Average
- Number of Tasks
- CPU Utilization
- Physical Memory Usage
- Swap Usage

---

## Process List

Summary Area ke neeche Process List hoti hai.

Yeh har running process ki information display karti hai.

Examples:

- PID
- User
- Priority
- Nice Value
- Memory Usage
- CPU Usage
- Process State
- Command Name

---

# 3. Pehli Line – Time, Uptime, Users Aur Load Average

Example:

```text
top - 15:20:41 up 2 days, 3:14, 2 users, load average: 0.20, 0.15, 0.10
```

---

## First-Line Fields

| Field | Matlab |
|--------|---------|
| Current Time | System ka current time |
| `up` | Server kitni dair se chal raha hai |
| Users | Is waqt kitne users login hain |
| Load Average | Pichlay 1, 5 aur 15 minutes ka average workload |

---

# Load Average Ko Samajhna

Example:

```text
load average: 0.20, 0.15, 0.10
```

Yeh teen values represent karti hain:

| Value | Time Period |
|------:|-------------|
| `0.20` | Last 1 Minute |
| `0.15` | Last 5 Minutes |
| `0.10` | Last 15 Minutes |

Load Average batata hai ke kitne tasks:

- CPU par run ho rahe hain.
- CPU ke wait mein hain.
- Uninterruptible I/O Sleep mein wait kar rahe hain.

---

# Important Note About Load Average

Load Average ko hamesha CPU ki tadaad ke mutabiq interpret karein.

Examples:

| Logical CPUs | Full Load Approx. |
|-------------:|------------------:|
| 1 | Around `1.00` |
| 2 | Around `2.00` |
| 4 | Around `4.00` |
| 8 | Around `8.00` |

Is liye har system par `2.00` se zyada Load Average ka matlab problem nahi hota.

CPU count check karne ke liye:

```bash
nproc
```

Ya:

```bash
lscpu
```

---

# 4. Doosri Line – Tasks Aur Process States

Example:

```text
Tasks: 210 total, 1 running, 208 sleeping, 0 stopped, 1 zombie
```

---

## Task-State Fields

| Field | Matlab |
|--------|---------|
| Total | Total processes ya tasks |
| Running | Jo is waqt CPU par run ho rahe hain |
| Sleeping | Kisi event ya resource ka wait kar rahe hain |
| Stopped | Suspend kiye gaye processes |
| Zombie | Complete ho chuke hain lekin parent ne cleanup nahi kiya |

---

# Process State Reminder

| State | Flag | Matlab |
|--------|------|---------|
| Running | `R` | Run ho raha hai ya CPU ka wait kar raha hai |
| Sleeping | `S` | Kisi event ka wait kar raha hai |
| Uninterruptible Sleep | `D` | Aksar Disk ya I/O ka wait |
| Stopped | `T` | Suspend kiya gaya |
| Zombie | `Z` | Process complete ho gaya lekin cleanup baqi hai |

---

# 5. Teesri Line – CPU Usage

Example:

```text
%Cpu(s): 2.0 us, 1.0 sy, 0.0 ni, 96.5 id, 0.5 wa, 0.0 hi, 0.0 si, 0.0 st
```

---

# CPU Fields

| Field | Matlab |
|--------|---------|
| `us` | User-space CPU Time |
| `sy` | Kernel-space CPU Time |
| `ni` | Nice Value wale process ka CPU Time |
| `id` | Idle CPU Time |
| `wa` | CPU I/O Wait Time |
| `hi` | Hardware Interrupt Time |
| `si` | Software Interrupt Time |
| `st` | Hypervisor ne Virtual Machine se liya hua CPU Time |

---

# `us` Ko Samajhna

`us` woh CPU time hai jo user applications use karti hain.

Examples:

- Shell Scripts
- Backup Scripts
- Web Applications
- Database Programs
- User Commands

Agar `us` high ho to CPU-intensive application chal rahi ho sakti hai.

---

# `sy` Ko Samajhna

`sy` Kernel ka CPU Time hota hai.

Examples:

- System Calls
- Device Drivers
- Filesystem Operations
- Networking
- Process Scheduling

Agar `sy` consistently high ho to Kernel heavy workload perform kar raha hota hai.

---

# `ni` Ko Samajhna

`ni` woh CPU time hai jo manually adjusted **Nice Value** wale processes use karte hain.

Nice Value scheduling priority ko affect karti hai.

---

# `id` Ko Samajhna

`id` ka matlab hai CPU idle hai.

Example:

```text
100.0 id
```

Matlab CPU bilkul free hai.

Agar `id` bohot kam ho to CPU busy hai.

---

# `wa` Ko Samajhna

`wa` ka matlab hai:

> **I/O Wait**

Yeh woh waqt hai jab CPU storage ya kisi I/O operation ka wait kar raha hota hai.

High `wa` indicate kar sakta hai:

- Slow Disk
- Heavy Backup
- Busy Storage
- Network Filesystem Delay
- Database I/O Pressure

Sirf `wa` dekh kar decision nahi lena chahiye.

Is ke saath yeh commands bhi use karein:

```bash
iostat
```

```bash
vmstat
```

```bash
iotop
```

---
---

# 6. Chauthi Line – Physical Memory Usage

Example:

```text
MiB Mem : 3895.2 total, 812.3 free, 1560.8 used, 1522.1 buff/cache
```

Yeh line system ki **Physical RAM** ki information dikhati hai.

---

## Memory Fields

| Field | Matlab |
|--------|---------|
| Total | Total installed RAM |
| Free | Bilkul free memory |
| Used | Jo memory currently use ho rahi hai |
| Buff/Cache | Linux ki cache aur buffers |

---

# Buffers Aur Cache

Linux free memory ko waste nahi karta.

Agar RAM khali ho to Linux usay cache ke liye use karta hai taa ke files jaldi access ho saken.

Agar kisi process ko memory chahiye hoti hai to Linux automatically cache release kar deta hai.

Is liye:

```text
Low Free Memory ≠ Memory Problem
```

Hamesha **Available Memory** ko bhi dekhein.

---

# 7. Paanchvi Line – Swap Memory

Example:

```text
MiB Swap: 2048 total, 2048 free, 0 used
```

Swap ek disk-based virtual memory hoti hai.

Jab RAM kam pad jaye to Linux swap use karta hai.

---

## Swap Fields

| Field | Matlab |
|--------|---------|
| Total | Total Swap Size |
| Free | Kitni Swap available hai |
| Used | Kitni Swap use ho rahi hai |

---

# Swap Kab Use Hoti Hai?

Linux Swap use karta hai jab:

- RAM full ho jaye
- Large applications chal rahi hon
- System memory pressure mein ho

Agar continuously Swap use ho rahi ho to RAM upgrade karna ya memory leak investigate karna zaroori ho sakta hai.

---

# 8. Process List

Summary Area ke neeche Process List hoti hai.

Yahan har running process ki detail hoti hai.

Example:

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
```

---

# Process List Columns

---

## PID

PID ka matlab hai:

> **Process ID**

Har process ka unique number hota hai.

Example:

```text
PID = 3521
```

Isi PID ki madad se process ko manage ya terminate kiya jata hai.

---

## USER

Yeh batata hai process kis user ne start kiya hai.

Example:

```text
root

student

apache

mysql
```

---

## PR

PR ka matlab hai:

> **Priority**

Kernel scheduling ke liye process ki priority.

Kam number ka matlab generally zyada priority hoti hai.

---

## NI

NI ka matlab hai:

> **Nice Value**

Nice Value process ki scheduling priority ko control karti hai.

Common Range:

```text
-20   Highest Priority

0     Default

19    Lowest Priority
```

---

## VIRT

VIRT ka matlab hai:

> **Virtual Memory**

Yeh total virtual memory hoti hai jo process use kar sakta hai.

Ismein include hota hai:

- Program Code
- Shared Libraries
- Swap
- Mapped Files

---

## RES

RES ka matlab hai:

> **Resident Memory**

Yeh actual RAM hai jo process iss waqt use kar raha hai.

---

## SHR

SHR ka matlab hai:

> **Shared Memory**

Yeh memory doosre processes ke saath share ki ja sakti hai.

Example:

- Shared Libraries
- Shared Memory Segments

---

## S

S process ki current state hoti hai.

Examples:

| State | Matlab |
|--------|---------|
| R | Running |
| S | Sleeping |
| D | Uninterruptible Sleep |
| T | Stopped |
| Z | Zombie |

---

## %CPU

Yeh batata hai process ne pichli refresh ke baad kitna CPU use kiya.

High CPU processes troubleshooting mein bohot important hoti hain.

---

## %MEM

Yeh batata hai process kitni RAM use kar raha hai.

Memory leak identify karne ke liye useful hai.

---

## TIME+

Yeh total CPU time hota hai jo process ne start hone ke baad use kiya.

---

## COMMAND

Yeh process ka naam ya executable hota hai.

Examples:

```text
sshd

httpd

mysqld

bash

firefox
```

---

# 🔬 Lab 2 – CPU Usage Observe Karein

Run karein:

```bash
top
```

CPU line observe karein.

Specially dekhein:

```text
us
sy
id
wa
```

Questions:

- CPU idle kitna hai?
- CPU user processes kitna use kar rahi hain?
- Kernel kitna CPU use kar raha hai?
- I/O Wait high hai ya nahi?

---

# 🔬 Lab 3 – Memory Observe Karein

`top` command mein Memory section dekhein.

Questions:

- Total RAM kitni hai?
- Free Memory kitni hai?
- Buff/Cache kitni hai?
- Available memory kitni hai?

---

# 🔬 Lab 4 – Swap Observe Karein

Swap line dekhein.

Questions:

- Swap configured hai?
- Swap use ho rahi hai?
- Agar use ho rahi hai to kitni?

---

# 🔬 Lab 5 – High CPU Process Identify Karein

`top` ke andar:

Press:

```text
P
```

Yeh CPU usage ke mutabiq processes ko sort karega.

Sab se upar wala process sab se zyada CPU use karega.

---

# 🔬 Lab 6 – High Memory Process Identify Karein

`top` ke andar:

Press:

```text
M
```

Processes memory usage ke mutabiq sort ho jayengi.

---

# 🔬 Lab 7 – Process Kill Karein

`top` ke andar:

Press:

```text
k
```

Phir:

- PID enter karein
- Signal number enter karein

Default:

```text
15
```

(SIGTERM)

---

# 🔬 Lab 8 – Refresh Delay Change Karein

Press:

```text
d
```

Example:

```text
5
```

Ab refresh har 5 seconds baad hogi.

---

# 🔬 Lab 9 – Specific User Ke Processes

Command:

```bash
top -u student
```

Sirf student user ke processes show honge.

---

# 🔬 Lab 10 – Exit

Exit karne ke liye:

```text
q
```

---

# 🧪 Practice Exercises

---

## Exercise 1

`top` start karein.

```bash
top
```

---

## Exercise 2

Load Average identify karein.

---

## Exercise 3

CPU Idle Percentage note karein.

---

## Exercise 4

Memory Usage note karein.

---

## Exercise 5

Swap Usage check karein.

---

## Exercise 6

CPU ke hisaab se sort karein.

```text
P
```

---

## Exercise 7

Memory ke hisaab se sort karein.

```text
M
```

---

## Exercise 8

Specific user ke processes dekhein.

```bash
top -u root
```

---

## Exercise 9

Refresh delay change karein.

```text
d
```

---

## Exercise 10

`top` exit karein.

```text
q
```

---

# 🔧 Troubleshooting Scenarios

---

## Scenario 1

System bohot slow hai.

Command:

```bash
top
```

Check karein:

- Load Average
- CPU Usage
- Memory
- Swap

---

## Scenario 2

CPU 100% use ho rahi hai.

Press:

```text
P
```

High CPU process identify karein.

---

## Scenario 3

Memory full lag rahi hai.

Observe karein:

```text
RES

%MEM

Available Memory
```

---

## Scenario 4

High I/O Wait

Check:

```text
wa
```

Agar value continuously high ho to storage ya backup activity investigate karein.

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `top` | Real-time Process Monitor |
| `top -u user` | User-specific Processes |
| `P` | CPU Sort |
| `M` | Memory Sort |
| `k` | Process Kill |
| `d` | Refresh Delay |
| `q` | Exit |

---

# 📖 Key Takeaways

- `top` Linux ka sab se important monitoring tool hai.
- Summary Area system health batata hai.
- Process List running processes ki detail show karti hai.
- CPU, Memory aur Swap ko regularly monitor karna chahiye.
- Load Average ko CPU cores ke mutabiq interpret karein.
- High `wa` storage ya I/O issue indicate kar sakta hai.
- `P` aur `M` troubleshooting ke liye bohot useful shortcuts hain.

---

# 💡 Yaad Rakhein

> **`top` ko Linux ka Live Dashboard samjhein.**
>
> - Dashboard aap ko real-time system ki health dikhata hai.
> - CPU usage
> - Memory usage
> - Swap
> - Running Processes
> - Load Average
>
> **Golden Rule:**
>
> ```text
> Slow System
>      │
>      ▼
> Run top
>      │
>      ▼
> Check CPU
>      │
>      ▼
> Check Memory
>      │
>      ▼
> Check Load Average
>      │
>      ▼
> Identify Problem Process
> ```
>
> Har Linux System Administrator ko `top` command ka output achi tarah samajhna chahiye kyun ke troubleshooting ka pehla qadam aksar `top` hi hota hai.