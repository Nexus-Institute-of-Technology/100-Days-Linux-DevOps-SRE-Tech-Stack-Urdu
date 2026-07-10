# MODULE 08 – Monitor and Manage Linux Processes
> **Linux Process States Aur Un Ka Life Cycle (Roman Urdu)**

> **DAY 65**  
> **JULY 11TH, 2026**

---

# 🎯 Learning Objectives

Is lesson mein aap seekhenge:

- Process State kya hoti hai.
- Process ek state se doosri state mein kyun jata hai.
- Linux Process Life Cycle.
- Runnable, Running, Sleeping, Stopped aur Zombie Processes ka farq.
- Linux Kernel processes ko kis tarah manage karta hai.
- Common Process-State Flags jaise `R`, `S`, `D`, `T`, aur `Z` ka matlab.
- Parent aur Child Processes execution ke dauran kis tarah interact karte hain.

---

# 📖 Introduction

Ek **Process** kisi running program ki instance hota hai.

Isay is tarah bhi define kiya ja sakta hai:

> **Executable instructions ka ek set jo memory mein load hota hai aur Linux Kernel usay manage karta hai.**

Apni life cycle ke dauran ek process hamesha ek hi condition mein nahi rehta.

Woh mukhtalif **Process States** se guzarta hai, is baat par depend karta hai ke woh:

- CPU time ka wait kar raha hai.
- Is waqt execute ho raha hai.
- Kisi resource ka wait kar raha hai.
- Temporary tor par stop hua hai.
- Apni execution complete kar raha hai.
- Parent ke exit status collect karne ka wait kar raha hai.

Linux **Kernel** ki zimmedari hai:

- Processes create karna.
- Processes schedule karna.
- CPU time allocate karna.
- Process states track karna.
- Processes suspend aur resume karna.
- Processes terminate karna.
- Process table ki entries clean karna.

---

# 1. Process State Kya Hai?

**Process State** process ki current condition ko represent karti hai.

Misal ke taur par process:

- CPU par run kar raha ho.
- Run karne ke liye ready ho.
- Kisi event ka wait karte hue sleep mein ho.
- Signal ki wajah se stopped ho.
- Finish ho chuka ho lekin cleanup ka wait kar raha ho.

Kernel har process aur us ki current state ka record rakhta hai.

---

# 📊 Linux Process-State Life Cycle

Neeche diya gaya diagram dikhata hai ke Linux Process mukhtalif states ke darmiyan kis tarah move karta hai.

```markdown
![Linux Process State Life Cycle](image(524).png)
```

---

# Process Life Cycle Ka Overview

```text
Process Create Hota Hai
      │
      ▼
New
      │
      ▼
Runnable / Ready
      │
      ▼
Running
      │
      ├── Event ya Resource Ka Wait ──► Sleeping
      │                                 │
      │                                 └── Event Complete ──► Runnable
      │
      ├── Suspend ──► Stopped
      │                │
      │                └── Resume ──► Runnable
      │
      ├── System Call ──► Kernel Running
      │                   │
      │                   └── Return ──► User Running
      │
      └── Exit ──► Zombie
                     │
                     └── Parent Reap Kare ──► Removed
```

---

# 2. New State

**New State** process ki pehli state hoti hai.

Process is state mein us waqt aata hai jab woh abhi abhi create hua ho.

Naya process aam tor par is ke zariye create hota hai:

```text
fork()
```

Is stage par:

- Process create ho chuka hota hai.
- Usay PID assign hota hai.
- Kernel us ka process structure banata hai.
- Abhi us ne normal execution start nahi ki hoti.

Is ke baad process **Runnable State** mein schedule hota hai.

---

# 3. Runnable Ya Ready State

**Runnable** ya **Ready State** mein process execute hone ke liye tayyar hota hai.

Lekin mumkin hai ke woh abhi CPU par apni bari ka wait kar raha ho.

Is state mein:

- Process ke paas run karne ke liye zaroori cheezen mojood hoti hain.
- Woh CPU run queue mein wait karta hai.
- Kernel Scheduler decide karta hai ke usay CPU time kab milega.

Is state ka flag hai:

```text
R
```

---

# Runnable Process Ko Wait Kyun Karna Parta Hai?

Ek hi waqt mein bohot se processes ready ho sakte hain.

Misal:

```text
Process A
Process B
Process C
Process D
```

CPU cores ki tadaad ke mutabiq sirf limited processes ek hi waqt mein run kar sakte hain.

Linux Scheduler decide karta hai ke kaunsa process chalega, factors jaise:

- Process Priority
- Scheduling Policy
- Available CPU
- Nice Value
- Previous CPU Usage

---

# 4. Running State

Process **Running State** mein tab aata hai jab CPU us ki instructions execute karna shuru karta hai.

Process state flag:

```text
R
```

Linux aksar `R` dono ke liye use karta hai:

- Running
- Runnable

Is liye `R` flag wala process:

- CPU par actively execute ho raha ho sakta hai.
- Ya CPU run queue mein wait kar raha ho sakta hai.

---

# User Mode Aur Kernel Mode

Running process do mukhtalif modes mein execute kar sakta hai.

## User Mode

Process normal application instructions execute karta hai.

Examples:

- `pwd` run karna
- Shell script chalana
- Firefox run karna
- Text editor use karna

---

## Kernel Mode

Process Kernel Mode mein tab jata hai jab woh operating system ki protected service request karta hai.

Examples:

- File read karna
- Disk par write karna
- Memory allocate karna
- Network connection open karna
- Naya process create karna
- System call perform karna

Flow:

```text
User Mode Mein Running
        │
        ▼
System Call
        │
        ▼
Kernel Mode Mein Running
        │
        ▼
Return
        │
        ▼
User Mode Mein Running
```

---

# 5. Sleeping Ya Waiting State

Process **Sleeping** ya **Waiting State** mein tab jata hai jab woh foran continue nahi kar sakta.

Ye aam tor par tab hota hai jab process wait kar raha ho:

- Disk Input/Output
- Network Data
- User Input
- Child Process
- Timer
- Signal
- Hardware Device
- Locked Resource

Wait karte waqt process ko CPU use karne ki zarurat nahi hoti.

Kernel CPU kisi aur process ko de deta hai.

---

# Sleeping Process Ki Example

Suppose backup process ek bari file disk par write kar raha hai.

Storage device ko write complete karne mein waqt lag sakta hai.

Is dauran:

```text
Process Running
      │
      ▼
Disk I/O Request
      │
      ▼
Process Sleep Karta Hai
      │
      ▼
Disk Operation Complete
      │
      ▼
Process Dobarah Runnable Ho Jata Hai
```

---

# 6. Interruptible Sleep

**Interruptible Sleeping** process kisi event ya condition ka wait kar raha hota hai.

Is ka state flag hai:

```text
S
```

Examples:

- Keyboard input ka wait
- Network traffic ka wait
- Timer ka wait
- Signal ka wait
- Child Process ka wait

Interruptible Sleeping Process ko jagaya ja sakta hai:

- Required event se
- Signal se
- System request complete hone se

Jagane ke baad process dobara **Runnable State** mein aata hai.

---

# 7. Uninterruptible Sleep

**Uninterruptible Sleeping** process aam tor par kisi important Kernel ya hardware operation ka wait kar raha hota hai.

Is ka state flag hai:

```text
D
```

Typical examples:

- Disk I/O
- Storage Device Response
- Network Filesystem Operation
- Kuch Kernel Operations

`D` state wala process aam tor par normal signals ka response nahi deta jab tak operation complete na ho.

Ye data corruption ya unpredictable device condition se bachane ke liye hota hai.

---

# 8. Killable Sleep

**Killable Sleeping** process ka state flag hai:

```text
K
```

Ye Uninterruptible Sleep jaisa hota hai, lekin fatal signals ka response de sakta hai.

Is se zarurat par process ko terminate kiya ja sakta hai jab ke Kernel operation ki protection bhi rehti hai.

Ye state zyada tar Kernel-level activity ke saath related hoti hai.

---

# 9. Idle Ya Report-Idle State

State flag:

```text
I
```

aam tor par **Idle Kernel Thread** ko represent karta hai.

Ye sleeping Kernel threads hote hain jo is waqt koi kaam perform nahi kar rahe hote.

Kernel system load calculate karte waqt inhein alag treat karta hai.

---

# 📊 Linux Process-State Flags

Neeche di gayi table common Linux process states ko summarize karti hai.

```markdown
![Linux Process State Flags](image(525).png)
```

---

# Process-State Reference Table

| State | Flag | Matlab |
|-------|------|--------|
| Running ya Runnable | `R` | Process CPU par execute ho raha hai ya run queue mein wait kar raha hai |
| Interruptible Sleep | `S` | Event ka wait kar raha hai aur signal se jagaya ja sakta hai |
| Uninterruptible Sleep | `D` | I/O ya Kernel resource ka wait aur aam tor par signals ka response nahi deta |
| Killable Sleep | `K` | `D` jaisa, lekin fatal signals ka response deta hai |
| Idle Kernel Thread | `I` | Idle Kernel thread jo normal system load mein count nahi hota |
| Stopped | `T` | Process suspend ya stop hua hai |
| Traced | `T` ya `t` | Process debug ya trace ho raha hai |
| Zombie | `Z` | Process exit ho chuka hai lekin Parent ne status collect nahi kiya |
| Dead | `X` | Process completely clean ho chuka hai aur aam tor par listing mein nazar nahi aata |

---

# 10. Stopped State

Process **Stopped State** mein tab jata hai jab us ki execution suspend kar di jaye.

State flag:

```text
T
```

Process stop ho sakta hai:

- User ki wajah se
- Kisi doosre process ki wajah se
- Signal ki wajah se
- Debugger ki wajah se
- Job Control Commands ki wajah se

Misal ke taur par:

```text
Ctrl + Z
```

foreground process ko suspend kar sakta hai.

Process memory mein rehta hai lekin execute nahi karta jab tak resume na ho.

---

# Suspend Aur Resume Flow

```text
Running Ya Runnable
        │
        ▼
Suspend Signal
        │
        ▼
Stopped
        │
        ▼
Resume Signal
        │
        ▼
Runnable
```

---

# 11. Traced State

Jo process debugging tool ke zariye inspect ho raha ho woh Traced State mein aa sakta hai.

State:

```text
T
```

ya kabhi:

```text
t
```

Debugging tools ki examples:

```bash
strace
```

aur:

```bash
gdb
```

Debugger process ki execution inspect karte waqt usay temporarily stop kar sakta hai.

---

# 12. Exit Ya Termination State

Process apna kaam complete karne ke baad **Exit State** mein jata hai.

Termination ke dauran:

- Open files close hoti hain.
- Memory release hoti hai.
- File descriptors remove hote hain.
- CPU resources free hote hain.
- Exit status save hota hai.
- Parent Process ko notification milti hai.

Completely remove hone se pehle process temporary **Zombie** ban sakta hai.

---

# 13. Zombie State

**Zombie Process** woh process hai jo execution complete kar chuka ho lekin process table mein abhi bhi us ki entry mojood ho.

State flag:

```text
Z
```

Zombie Process is liye rehta hai kyun ke Parent Process ne abhi tak us ka exit status collect nahi kiya.

---

# Zombie Process Flow

```text
Child Process Running
        │
        ▼
Child Exit Karta Hai
        │
        ▼
Exit Status Save Hota Hai
        │
        ▼
Zombie State
        │
        ▼
Parent wait() Call Karta Hai
        │
        ▼
Process Entry Remove Ho Jati Hai
```

---

# Kya Zombie Process Resources Consume Karta Hai?

Zombie Process:

- Instructions execute nahi karta.
- Normal CPU time use nahi karta.
- Normal process memory retain nahi karta.
- Process table mein ek entry occupy karta hai.
- Temporary tor par PID aur exit status retain karta hai.

Ek ya do temporary zombies aam tor par serious issue nahi hote.

Lekin bohot zyada persistent zombies is baat ki nishani ho sakte hain ke Parent Process child exit statuses collect nahi kar raha.

---

# 14. Zombie Process Ko Reap Karna

Parent Process Child ka exit status collect karke Zombie entry remove karta hai.

Is operation ko kehte hain:

> **Reaping**

Ye aam tor par in system calls se hota hai:

```text
wait()
```

ya:

```text
waitpid()
```

Child reap hone ke baad:

- PID release ho jata hai.
- Process table entry remove ho jati hai.
- Process listing mein nazar nahi aata.

---

# 15. Dead State

State flag:

```text
X
```

completely dead process ko represent karta hai.

Is stage par:

- Parent exit status collect kar chuka hota hai.
- Process table entry release ho chuki hoti hai.
- PID dobara use ho sakta hai.
- Process aam tor par `ps` commands mein nazar nahi aata.

---

# 16. Process Preemption

Running process ko us ka kaam complete hone se pehle CPU se hata diya ja sakta hai.

Isay kehte hain:

> **Preemption**

Kernel process ko preempt kar sakta hai jab:

- CPU time slice khatam ho jaye.
- Higher-priority process runnable ho jaye.
- Scheduler kisi aur process ko select kare.
- Process khud wait kare.

Process phir dobara **Runnable State** mein chala jata hai.

---

# Preemption Flow

```text
Running
   │
   ▼
Time Slice Khatam Ya Higher-Priority Process Aaye
   │
   ▼
Runnable
   │
   ▼
Dobarah CPU Ka Wait
```

---

# 17. Complete Process Life Cycle

Typical Process Life Cycle:

```text
fork()
   │
   ▼
New
   │
   ▼
Runnable
   │
   ▼
Running
   │
   ├── Preempted ──────► Runnable
   │
   ├── I/O Ka Wait ────► Sleeping
   │                       │
   │                       └── Event Complete ──► Runnable
   │
   ├── Suspended ──────► Stopped
   │                       │
   │                       └── Resume ──────────► Runnable
   │
   └── Exit ───────────► Zombie
                           │
                           └── Reaped ─────────► Dead/Removed
```

---

# 18. Real-World Example: `pwd` Run Karna

Suppose user ye command run karta hai:

```bash
pwd
```

Process ka path ho sakta hai:

```text
Bash Child Process Create Karta Hai
        │
        ▼
New
        │
        ▼
Runnable
        │
        ▼
Running
        │
        ▼
pwd Current Directory Read Karta Hai
        │
        ▼
Output Display Hoti Hai
        │
        ▼
Process Exit Karta Hai
        │
        ▼
Parent Exit Status Collect Karta Hai
        │
        ▼
Process Remove Ho Jata Hai
```

---

# 19. Real-World Example: Backup Process

Suppose backup script chal rahi hai:

```bash
./backup.sh
```

Process:

1. New State mein start hota hai.
2. Runnable State mein jata hai.
3. CPU milne par Running State mein aata hai.
4. Child Processes create kar sakta hai.
5. Disk I/O ka wait karta hai.
6. Sleeping State mein jata hai.
7. Disk I/O complete hone par dobara Runnable hota hai.
8. Dobarah run karta hai.
9. Backup complete hone par exit karta hai.
10. Temporary Zombie banta hai.
11. Parent usay reap karta hai.

---

# 20. Parent Process Ka Child Ka Wait Karna

Parent Process Child ke complete hone ka wait kar sakta hai.

Example:

```text
Parent Process
      │
      ├── Child Create Karta Hai
      │
      ▼
Parent Wait Ya Sleep Karta Hai
      │
      ▼
Child Backup Perform Karta Hai
      │
      ▼
Child Exit Karta Hai
      │
      ▼
Parent Exit Status Collect Karta Hai
```

Ye bilkul normal process behavior hai.

---

# 21. Process States Important Kyun Hain?

Process States samajhne se Linux Administrators ko madad milti hai:

- CPU-intensive processes identify karne mein.
- Blocked I/O operations detect karne mein.
- Suspended jobs dhoondhne mein.
- Zombie Processes diagnose karne mein.
- System slowness investigate karne mein.
- Unresponsive services troubleshoot karne mein.
- Load Average samajhne mein.
- Ye determine karne mein ke process running hai ya wait kar raha hai.

---

# 22. Process States Dekhne Ke Commands

Common commands:

```bash
ps
```

```bash
ps aux
```

```bash
ps -ef
```

```bash
ps -eo pid,ppid,stat,cmd
```

```bash
top
```

```bash
htop
```

Example:

```bash
ps -eo pid,ppid,stat,cmd
```

Ye display karta hai:

- PID
- PPID
- Process State
- Command

---

# `STAT` Column Ko Samjhein

Example:

```text
PID   PPID  STAT  CMD
1     0     Ss    /usr/lib/systemd/systemd
850   1     S     /usr/sbin/sshd
921   850   R+    ps -eo pid,ppid,stat,cmd
```

`STAT` column ka pehla character main process state dikhata hai.

Examples:

| STAT | Matlab |
|------|--------|
| `R` | Running ya Runnable |
| `S` | Interruptible Sleep |
| `D` | Uninterruptible Sleep |
| `T` | Stopped |
| `Z` | Zombie |
| `I` | Idle Kernel Thread |

Extra characters process ki mazeed information dete hain.

---

# 📌 Quick Revision

| Process State | Flag | Matlab |
|---------------|------|--------|
| New | — | Process abhi create hua hai |
| Runnable | `R` | Ready hai aur CPU ka wait kar raha hai |
| Running | `R` | CPU par execute ho raha hai |
| Interruptible Sleep | `S` | Wait kar raha hai aur signals ka response de sakta hai |
| Uninterruptible Sleep | `D` | I/O ka wait aur aam tor par interrupt nahi hota |
| Killable Sleep | `K` | Kernel sleep jo fatal signals accept karta hai |
| Idle | `I` | Idle Kernel Thread |
| Stopped | `T` | Suspended process |
| Traced | `T` ya `t` | Debug ho raha process |
| Zombie | `Z` | Finish ho chuka lekin reap nahi hua |
| Dead | `X` | Completely remove ho chuka process |

---

# 📖 Key Takeaways

- Process apni life cycle mein mukhtalif states se guzarta hai.
- Naya process sab se pehle New State mein hota hai.
- Runnable Process ready hota hai lekin CPU ka wait kar sakta hai.
- Running Process actively execute ho raha hota hai.
- Sleeping Processes resources ya events ka wait karte hain.
- `S` ka matlab Interruptible Sleep hai.
- `D` ka matlab Uninterruptible Sleep hai.
- `T` ka matlab Stopped ya Traced hai.
- `Z` ka matlab Zombie hai.
- Zombie Process tab tak rehta hai jab tak Parent us ka exit status collect nahi karta.
- Linux Kernel process scheduling aur state transitions control karta hai.

---

# 💡 Yaad Rakhein

> **Process States ko un students ki tarah samjhein jo ek computer use karne ka wait kar rahe hain.**
>
> - **New** – Student abhi lab mein aya hai.
> - **Runnable** – Student ready hai aur computer ka wait kar raha hai.
> - **Running** – Student is waqt computer use kar raha hai.
> - **Sleeping** – Student kisi file, device ya instruction ka wait kar raha hai.
> - **Stopped** – Instructor ne student ko temporary pause kiya hai.
> - **Zombie** – Student task complete kar chuka hai lekin attendance update nahi hui.
> - **Dead** – Task completely close ho gaya aur record remove ho gaya.
>
> **Golden Flow:**
>
> ```text
> New
>  │
>  ▼
> Runnable
>  │
>  ▼
> Running
>  │
>  ├──► Sleeping ───► Runnable
>  │
>  ├──► Stopped ────► Runnable
>  │
>  └──► Zombie ─────► Removed
> ```