# MODULE 08 – Process Ko Samajhna (Kaise Kaam Karta Hai)
> **Linux Process States Aur Process Life Cycle**

---

# 🎯 Learning Objectives

Is lesson mein aap seekhenge:

- Linux Process kya hota hai.
- Process execution ke dauran mukhtalif states se kaise guzarta hai.
- Linux Process Life Cycle.
- Linux Kernel process ko kaise manage karta hai.
- New State.
- Ready State.
- Running State.
- Waiting (Blocked) State.
- Sleeping State.
- Exit State.
- Zombie State.
- CPU Scheduling ka basic concept.

---

# 📖 Introduction

Hum pehle hi seekh chuke hain ke:

> **Process ek running program ki instance hota hai.**

Ya doosre alfaaz mein:

> **Executable instructions ka ek set jo memory mein load hota hai aur CPU execute karta hai.**

Har process apni execution ke dauran mukhtalif states se guzarta hai.

In states ko samajhna Linux System Administrator ke liye bohot zaroori hai, khaas taur par troubleshooting aur performance monitoring ke liye.

Linux ka **Kernel** system ke tamam processes ko manage karta hai. Yeh har process ka record rakhta hai, CPU allocate karta hai, resources manage karta hai aur process ko ek state se doosri state mein move karta hai.

---

# Linux Process Life Cycle

<img src="../../.github/assets/Process States.png" width="700">

---
Har Linux process aam tor par neeche diye gaye states se guzarta hai:

```text
           fork()
             │
             ▼
         New State
             │
             ▼
        Ready State
             │
             ▼
       Running State
        /          \
       /            \
Waiting/Sleep     Exit
       │             │
       ▼             ▼
    Running       Zombie
```

---

# 1. New State

**New State** process ki sab se pehli state hoti hai.

Jab koi process pehli dafa create hota hai to woh isi state mein hota hai.

Is stage par Linux Kernel:

- Process create karta hai.
- Process ID (PID) assign karta hai.
- Memory allocate karta hai.
- Zaroori system resources prepare karta hai.

Is waqt process abhi execute nahi ho raha hota.

---

# 2. Ready State

Jab process successfully create ho jata hai to woh **Ready State** mein chala jata hai.

Ready State ka matlab hai:

- Process execution ke liye bilkul tayyar hai.
- CPU ka wait kar raha hai.
- Kernel Scheduler decide karega ke kis process ko pehle CPU milega.

Ek waqt mein bohot saare processes Ready Queue mein maujood ho sakte hain.

Kernel CPU Scheduling Algorithm aur process priority ki bunyaad par decide karta hai ke kis process ko execute karna hai.

---

# 3. Running State

Jab Kernel Scheduler kisi process ko CPU allocate karta hai to woh **Running State** mein aa jata hai.

Ab CPU us process ki instructions execute karta hai.

Misal ke taur par agar user ye command chalata hai:

```bash
pwd
```

Linux is command ke liye ek process create karta hai.

Ye process:

- CPU use karta hai.
- Memory access karta hai.
- Instructions execute karta hai.
- Current Working Directory display karta hai.

Isi tarah:

```bash
ls
```

ya

```bash
tar -cvf backup.tar /etc
```

Ye tamam commands Running Process ki examples hain.

---

# CPU Scheduling

Ready Queue mein ek waqt par bohot saare processes wait kar rahe hote hain.

Kernel ka CPU Scheduler decide karta hai:

- Kis process ko pehle CPU diya jaye.
- Kitni dair tak woh process chale.
- Kab doosre process ko CPU diya jaye.

Higher priority wale processes ko zarurat ke mutabiq pehle execute kiya ja sakta hai.

---

# 4. Waiting (Blocked) State

Kabhi kabhi Running Process ko kisi operation ka wait karna padta hai.

Misal ke taur par:

- Disk se data read karna.
- File par data write karna.
- Network response ka wait karna.
- User input ka wait karna.
- Child Process ke complete hone ka wait karna.

Aisi surat mein process:

> **Waiting (Blocked) State**

mein chala jata hai.

Jab tak woh wait kar raha hota hai, CPU kisi doosre process ko de diya jata hai.

---

# Example: Waiting State

Suppose ek backup process disk par data write kar raha hai.

Disk operation complete hone tak process:

```text
Running
      │
      ▼
Waiting
```

state mein rahega.

Jaise hi disk operation complete hogi, process dobara Running State mein aa jayega.

---

# Sleeping State

Sleeping bhi Waiting State ki ek qisam hai.

Misal ke taur par:

```bash
sleep 30
```

ya Parent Process Child Process ka wait kar raha ho.

Is waqt process temporary execution rok deta hai aur required event ka wait karta hai.

---

# 5. Exit (Termination) State

Jab process apna kaam mukammal kar leta hai to woh **Exit State** ya **Termination State** mein chala jata hai.

Kernel phir:

- Memory release karta hai.
- CPU resources free karta hai.
- Open files close karta hai.
- Allocated resources release karta hai.

Is ke baad process ka execution complete ho jata hai.

---

# Zombie Process

Kabhi kabhi process execution complete kar leta hai lekin us ka Parent Process abhi tak us ka exit status receive nahi karta.

Is arsay ke dauran process:

> **Zombie Process**

kehlata hai.

Zombie Process:

- CPU use nahi karta.
- Memory bohot kam use karta hai.
- Sirf Process Table mein ek entry ki surat mein mojood hota hai.

Jab Parent Process us ka exit status read kar leta hai to Zombie Process remove ho jata hai.

---

# Common Linux Process States

Linux mein aam tor par ye process states dekhi jati hain.

| State | Matlab |
|--------|--------|
| New | Process abhi create hua hai. |
| Ready | CPU milne ka wait kar raha hai. |
| Running | CPU process ko execute kar raha hai. |
| Waiting / Blocked | Kisi event ya I/O ka wait kar raha hai. |
| Sleeping | Temporary pause state mein hai. |
| Stopped | Process ko rok diya gaya hai. |
| Zombie | Process complete ho chuka hai lekin Parent ne exit status receive nahi kiya. |
| Exit | Process successfully terminate ho chuka hai. |

---

# Additional Linux Process State Codes

`ps` aur `top` command chalane par aap ye state codes dekh sakte hain.

| Code | Matlab |
|------|--------|
| R | Running ya Runnable Process |
| S | Interruptible Sleep |
| D | Uninterruptible Sleep (aksar Disk I/O ka wait) |
| I | Idle Kernel Thread |
| T | Stopped Process |
| t | Traced ya Debugged Process |
| Z | Zombie Process |
| X | Dead Process (bohot kam nazar aata hai) |

---

# Process State Flow

```text
fork()
   │
   ▼
New
   │
   ▼
Ready
   │
   ▼
Running
   │
   ├─────────────┐
   ▼             │
Waiting/Sleep    │
   │             │
   └─────────────┘
         │
         ▼
      Running
         │
         ▼
       Exit
         │
         ▼
      Zombie
```

---

# Linux Administrator Ke Liye Process States Kyun Important Hain?

Linux System Administrator ko Process States samajhna is liye zaroori hai kyun ke in ki madad se woh:

- Slow system troubleshoot kar sakta hai.
- Blocked processes identify kar sakta hai.
- Zombie processes detect kar sakta hai.
- CPU scheduling analyze kar sakta hai.
- System performance monitor kar sakta hai.
- Disk aur I/O bottlenecks identify kar sakta hai.
- Applications ke behavior ko samajh sakta hai.

---

# 📌 Quick Revision

| State | Kaam |
|--------|------|
| New | Process create hota hai. |
| Ready | CPU milne ka wait karta hai. |
| Running | CPU process ko execute karta hai. |
| Waiting | I/O ya kisi event ka wait karta hai. |
| Sleeping | Temporary pause state. |
| Exit | Execution complete ho chuki hoti hai. |
| Zombie | Parent Process exit status receive karne ka wait karta hai. |

---

# 📖 Key Takeaways

- Har process execution ke dauran mukhtalif states se guzarta hai.
- Linux Kernel tamam process state transitions ko manage karta hai.
- Naya process pehle Ready Queue mein jata hai.
- CPU Scheduler decide karta hai ke kaunsa process execute hoga.
- Running Process zarurat par Waiting ya Sleeping State mein ja sakta hai.
- Kaam complete hone ke baad process Exit State mein chala jata hai.
- Zombie Process temporary state hoti hai jab tak Parent Process exit status collect nahi karta.
- Process States ko samajhna Linux Administration aur troubleshooting ke liye bohot zaroori hai.

---

# 💡 Yaad Rakhein

> **Process ko bank ke customer ki tarah samjhein.**
>
> - **New** → Customer bank mein enter karta hai.
> - **Ready** → Line mein apni baari ka wait karta hai.
> - **Running** → Teller us ka kaam kar raha hota hai.
> - **Waiting** → Kisi document ya approval ka wait hota hai.
> - **Running Again** → Kaam dobara shuru hota hai.
> - **Exit** → Customer bank se nikal jata hai.
> - **Zombie** → Customer chala gaya hai lekin paperwork abhi complete hona baqi hai.

---

## Golden Rule

```text
New
  │
  ▼
Ready
  │
  ▼
Running
  │
  ▼
Waiting
  │
  ▼
Running
  │
  ▼
Exit
  │
  ▼
Zombie
```