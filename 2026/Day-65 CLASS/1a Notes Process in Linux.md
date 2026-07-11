# MODULE 08 – Monitor and Manage Linux Processes
> **Linux Processes Ka Introduction (Roman Urdu)**

> **DAY 65**
> **JULY 11TH, 2026**

---

# 🎯 Learning Objectives

Is module mein aap seekhenge:

- Linux Process kya hota hai?
- Process kis tarah create hota hai?
- Process Life Cycle.
- Parent aur Child Processes.
- Process Forking.
- Process IDs (PID & PPID).
- Process Environment.
- Process States.
- Linux System Administrator ke liye Process Management kyun important hai.

---

# 📖 Introduction

Linux system par jo bhi kaam hota hai woh **Process** ki shakal mein hota hai.

Jab bhi aap koi command execute karte hain, koi program start karte hain ya koi script run karte hain, Linux us kaam ko perform karne ke liye ek ya zyada processes create karta hai.

Linux ka **Kernel** processes ko create, schedule, manage aur terminate karta hai.

Agar processes na hon to Linux koi bhi kaam perform nahi kar sakta.

---

# 1. Process Kya Hai?

**Process** hota hai:

> **Kisi running program ki ek instance.**

Ya

> **Ek executable program ki running instance jo operating system ne launch ki ho.**

Simple alfaaz mein:

- Disk par mojood Program → **Static**
- Memory mein chal raha Program → **Process**

---

# Process Ki Example

Suppose aap ye command chalate hain:

```bash
pwd
```

Background mein kya hota hai?

1. Bash command receive karta hai.
2. Linux ek naya process create karta hai.
3. Kernel us process ko allocate karta hai:
   - CPU Time
   - Memory
4. Process `pwd` command execute karta hai.
5. Current Working Directory display hoti hai.
6. Process apna kaam complete karke exit ho jata hai.

---

# Ek Aur Example

Suppose aap ek backup script execute karte hain.

```bash
./backup.sh
```

Linux ye kaam karta hai:

- Ek process create karta hai.
- CPU allocate karta hai.
- RAM allocate karta hai.
- Script execute karta hai.
- Zarurat ho to child processes create karta hai.
- Execution complete karta hai.
- Resources release kar deta hai.

---

# Process Ki Definition

Process simply hota hai:

> **Executable instructions ka ek set jo memory mein load hota hai aur CPU execute karta hai.**

Har process ko Linux Kernel system resources provide karta hai.

---

# 📊 Process Life Cycle Diagram

Neeche diya gaya diagram dikhata hai ke Linux process kis tarah create hota hai, child process banata hai aur aakhir mein exit hota hai.

```markdown
![Linux Process Life Cycle](image(523).png)
```

### Diagram Ki Explanation

1. Ek **Process** execution start karta hai.
2. Zarurat par woh **fork()** system call ki madad se ek **Child Process** create karta hai.
3. Child Process **exec()** ke zariye koi doosra program execute kar sakta hai.
4. Jab execution complete hoti hai to Child Process **Exit** karta hai.
5. Jab tak Parent us ka exit status collect nahi karta, Child temporarily **Zombie Process** ban jata hai.
6. Parent ke acknowledge karne ke baad Zombie entry remove ho jati hai.

---

# 2. Process Ke Components

Har process ke kuch important components hote hain.

---

## Memory Space

Har process ko apni alag memory allocate ki jati hai.

Is memory mein hota hai:

- Program Code
- Variables
- Stack
- Heap

---

## Security Information

Har process ke paas hota hai:

- Owner (User)
- Group
- Permissions
- Privileges

Ye decide karte hain ke process kya kya kaam kar sakta hai.

---

## Execution Threads

Har process mein ek ya zyada execution threads hote hain.

Ye threads program ki instructions execute karte hain.

---

## Process State

Har process kisi na kisi state mein hota hai.

Misal:

- Running
- Sleeping
- Waiting
- Stopped
- Zombie

In states ko hum aglay lessons mein detail se padhenge.

---

# 3. Process Environment

Har process ke saath ek **Environment** bhi hota hai.

Is mein shamil hota hai:

- Local Variables
- Environment Variables
- Current Working Directory
- Open Files
- File Descriptors
- Network Ports
- Allocated Resources

Ye tamam resources process ke saath us ki execution ke dauran attached rehte hain.

---

# 4. Process Creation (Fork)

Linux naye process create karta hai ek system call ke zariye jiska naam hai:

```text
fork()
```

**fork()** Parent Process ki copy bana kar ek naya process create karta hai.

Naye process ko kehte hain:

> **Child Process**

Aur original process ko kehte hain:

> **Parent Process**

---

# Parent Aur Child Process

Example:

```text
Parent Process
        │
     fork()
        │
        ▼
Child Process
```

Child Process shuru mein Parent Process ki bohot si properties inherit karta hai.

---

# 5. Child Process Kya Inherit Karta Hai?

Jab Child Process create hota hai to woh Parent Process se bohot si cheezen inherit karta hai.

Jaise:

- Security Identity
- User ID
- Group ID
- File Descriptors
- Environment Variables
- Resource Limits
- Program Code
- Current Working Directory

Us ke baad Child Process apna alag code bhi execute kar sakta hai.

---

# 6. Process IDs (PID)

Har process ko ek unique number diya jata hai jise kehte hain:

> **PID (Process ID)**

Example:

```text
PID = 3521
```

Linux PID ko use karta hai:

- Process ko track karne ke liye.
- Resources manage karne ke liye.
- Signals bhejne ke liye.
- Security maintain karne ke liye.

---

# Parent Process ID (PPID)

Har Child Process ke paas Parent Process ka ID bhi hota hai.

Usay kehte hain:

> **PPID (Parent Process ID)**

Example:

```text
Parent Process
PID = 1000

Child Process
PID = 1100
PPID = 1000
```

---

# 7. Har Process Child Process Bana Sakta Hai

Har running process ek ya zyada Child Processes create kar sakta hai.

Example:

```text
Process A
    │
    ├── Process B
    │
    ├── Process C
    │
    └── Process D
```

Aur har Child Process bhi mazeed Child Processes create kar sakta hai.

---

# 8. Agar Parent Process Terminate Ho Jaye To Kya Hota Hai?

Normally:

Agar Parent Process terminate ho jaye,

to Child Processes ya to terminate ho jate hain ya phir operating system unhein dobara kisi aur Parent ke saath attach kar deta hai.

Modern Linux systems mein orphan processes ko:

```text
systemd (PID 1)
```

adopt kar leta hai.

---

# 9. Linux Ka Sab Se Pehla Process

Modern Red Hat based Linux systems mein boot ke waqt sab se pehla process hota hai:

```text
systemd
```

Is ka:

```text
PID = 1
```

System ke tamam processes aakhir mein **systemd** se hi originate hote hain.

---

# 10. Parent Process Sleep Mode Mein

Aksar Parent Process wait karta hai jab us ka Child Process execute ho raha hota hai.

Example:

```text
Parent Process
      │
      ▼
Sleep (Waiting)
      │
      ▼
Child Executes
```

Parent Process Child ke complete hone tak wait state mein rehta hai.

---

# 11. Child Process Complete Hone Ke Baad

Jab Child Process finish hota hai:

- Resources release ho jate hain.
- Memory clean ho jati hai.
- Open files close ho jati hain.
- CPU resources free ho jate hain.

Completely remove hone se pehle process kuch dair ke liye:

> **Zombie Process**

ban jata hai.

---

# 12. Zombie Process

Zombie Process hota hai:

> **Aisa process jo apna execution complete kar chuka ho lekin Parent Process ne abhi tak us ka exit status collect na kiya ho.**

Zombie Process system resources bohot kam use karta hai.

Ye sirf Process Table mein ek entry occupy karta hai.

---

# 13. Processes Important Kyun Hain?

Linux Processes operating system ko ye kaam karne ki sahulat dete hain:

- Multiple programs ko ek saath chalana.
- CPU Scheduling.
- Memory ko efficiently allocate karna.
- Applications ko isolate karna.
- Multitasking improve karna.
- Security provide karna.

---

# Real-World Example

Suppose aap chalate hain:

```bash
firefox
```

Linux Firefox ka ek process create karta hai.

Firefox mazeed Child Processes create kar sakta hai:

- Rendering Engine
- GPU Process
- Network Process
- Audio Process

Har Child Process alag kaam perform karta hai.

---

# Process Creation Flow

```text
User Command Chalata Hai
        │
        ▼
Kernel Process Create Karta Hai
        │
        ▼
CPU Aur Memory Allocate Hoti Hai
        │
        ▼
Program Execute Hota Hai
        │
        ▼
Zarurat Par Child Processes Create Hote Hain
        │
        ▼
Execution Complete Hoti Hai
        │
        ▼
Resources Release Hote Hain
        │
        ▼
Zombie Entry Remove Ho Jati Hai
```

---

# 📌 Quick Revision

| Term | Matlab |
|------|--------|
| Process | Running Program ki Instance |
| PID | Process ID |
| PPID | Parent Process ID |
| Parent Process | Original Process |
| Child Process | `fork()` se create hota hai |
| fork() | Child Process create karta hai |
| exec() | Process ke andar naya program execute karta hai |
| Zombie | Exit hone ke baad Process Table mein temporary entry |
| systemd | Linux ka pehla process (PID 1) |

---

# 📖 Key Takeaways

- Har running program ek Process hota hai.
- Har Process ko Kernel CPU aur Memory allocate karta hai.
- Har Process ka apna unique PID hota hai.
- Child Processes `fork()` se create hote hain.
- Child Processes Parent ki bohot si properties inherit karte hain.
- Parent aksar Child Process ke complete hone ka wait karta hai.
- Finish hone ke baad Process temporarily Zombie ban sakta hai.
- Linux ke tamam Processes aakhir mein **systemd (PID 1)** se originate hote hain.

---

# 💡 Yaad Rakhein

> **Process ko ek company ke employee ki tarah samjhein.**
>
> - **Kernel** Company ka Manager hai.
> - **Program** Job Description hai.
> - **Process** Employee hai jo kaam kar raha hai.
> - **fork()** ek naya Employee (Child Process) hire karta hai.
> - **exec()** us Employee ko naya kaam deta hai.
> - Kaam complete hone ke baad Employee exit kar jata hai.
> - Jab tak HR (Parent Process) paperwork complete nahi karta, Employee temporarily **Zombie Process** ki surat mein nazar aata hai.
>
> **Golden Rule Yaad Rakhein:**
>
> ```text
> Program Disk Par
>        │
>        ▼
> Memory Mein Run Kare
>        │
>        ▼
> Process
> ```