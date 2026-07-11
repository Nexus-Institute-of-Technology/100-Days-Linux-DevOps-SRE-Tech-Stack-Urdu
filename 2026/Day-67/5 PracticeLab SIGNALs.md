# MODULE 08 – Practice Lab: Linux Process Signals
> **Hands-on Practice Lab – `SIGTSTP`, `SIGSTOP`, `SIGCONT`, `SIGTERM`, Aur `SIGKILL` (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- Linux Process Signals ko samajhna.
- `SIGTSTP` se process suspend karna.
- `SIGSTOP` se process ko forcefully stop karna.
- `SIGCONT` se stopped process ko resume karna.
- `SIGTERM` se process ko gracefully terminate karna.
- `SIGKILL` se process ko forcefully terminate karna.
- Signal names aur signal numbers dono use karna.
- `kill` command ke saath Job IDs aur Process IDs use karna.
- Process ko stop karne aur terminate karne ke darmiyan farq samajhna.

---

# 📖 Introduction

Linux running processes ke saath communicate karne ke liye **Signals** use karta hai.

Signal ek notification ya instruction hota hai jo process ko bheja jata hai taake woh koi specific action perform kare.

Signals ka istemal kiya ja sakta hai:

- Process suspend karne ke liye.
- Stopped process resume karne ke liye.
- Process terminate karne ke liye.
- Process ko forcefully kill karne ke liye.
- Service reload karne ke liye.
- Process ko kisi event ke bare mein notify karne ke liye.

Signals bhejne ke liye aam tor par `kill` command use hoti hai.

Apne naam ke bawajood, `kill` command hamesha process ko terminate nahi karti. Ye bohot se mukhtalif signals bhej sakti hai.

---

# 1. `kill` Command Ki Basic Syntax

Signal name use karte hue:

```bash
kill -SIGNAL PID
```

Example:

```bash
kill -SIGTERM 2502
```

Signal number use karte hue:

```bash
kill -NUMBER PID
```

Example:

```bash
kill -15 2502
```

Shell Job ID use karte hue:

```bash
kill -SIGNAL %JOB_NUMBER
```

Example:

```bash
kill -SIGCONT %1
```

---

# Job ID Aur Process ID Mein Farq

| Identifier | Example | Kis Ke Zariye Manage Hota Hai |
|------------|---------|--------------------------------|
| Job ID | `%1` | Current Shell |
| Process ID | `2502` | Linux Kernel |

Job ID ke saath percent sign lagana zaroori hai:

```text
%1
```

PID normal number ki tarah likha jata hai:

```text
2502
```

---

# 2. Available Signals Dekhein

Linux ke tamam available signals dekhne ke liye:

```bash
kill -l
```

Example output mein ye signals ho sakte hain:

```text
1) SIGHUP
2) SIGINT
9) SIGKILL
15) SIGTERM
18) SIGCONT
19) SIGSTOP
20) SIGTSTP
```

> **Important Note:** Signal numbers kuch systems aur architectures par mukhtalif ho sakte hain. Standard x86_64 Linux systems par aam tor par `SIGCONT` ka number 18, `SIGSTOP` ka 19 aur `SIGTSTP` ka 20 hota hai.

---

# Is Lab Ke Main Signals

| Signal | Common Number | Kaam |
|--------|---------------|------|
| `SIGTSTP` | `20` | Foreground process ko terminal se suspend kare |
| `SIGSTOP` | `19` | Process ko forcefully stop kare |
| `SIGCONT` | `18` | Stopped process ko resume kare |
| `SIGTERM` | `15` | Graceful termination request bheje |
| `SIGKILL` | `9` | Process ko forcefully terminate kare |

---

# 3. Practice Processes Create Karein

Do long-running background processes start karein:

```bash
sleep 1000 &
sleep 2000 &
```

Jobs aur un ke PIDs display karein:

```bash
jobs -l
```

Example Output:

```text
[1]- 2500 Running    sleep 1000 &
[2]+ 2502 Running    sleep 2000 &
```

Yahan:

- Job 1 ka PID `2500` hai.
- Job 2 ka PID `2502` hai.

Aap ke actual PIDs mukhtalif honge.

---

# 4. `SIGTSTP` – Terminal Stop Signal

`SIGTSTP` ka matlab hai:

> **Signal Terminal Stop**

Ye process ko suspend karta hai.

Common signal number:

```text
20
```

Ye wahi signal hai jo aam tor par is shortcut se bheja jata hai:

```text
Ctrl + Z
```

---

# `SIGTSTP` Ki Important Property

Process `SIGTSTP` ko:

- Catch kar sakta hai.
- Handle kar sakta hai.
- Ignore bhi kar sakta hai.

Matlab application decide kar sakti hai ke is signal ka kya response dena hai.

---

# 🔬 Lab 1 – `Ctrl + Z` Se Foreground Process Suspend Karein

Run karein:

```bash
sleep 500
```

Press karein:

```text
Ctrl + Z
```

Expected Output:

```text
[1]+  Stopped    sleep 500
```

Verify karein:

```bash
jobs
```

---

# `SIGTSTP` Manually Bhejein

Job ID use karte hue:

```bash
kill -SIGTSTP %1
```

Signal number use karte hue:

```bash
kill -20 %1
```

PID use karte hue:

```bash
kill -SIGTSTP 2500
```

---

# 5. `SIGSTOP` – Process Ko Forcefully Stop Karna

`SIGSTOP` process ko foran suspend karta hai.

Common signal number:

```text
19
```

Command:

```bash
kill -SIGSTOP PID
```

Ya:

```bash
kill -19 PID
```

---

# `SIGSTOP` Ki Important Property

`SIGSTOP` ko process:

- Catch nahi kar sakta.
- Handle nahi kar sakta.
- Block nahi kar sakta.
- Ignore nahi kar sakta.

Kernel process ko forcefully Stopped State mein bhej deta hai.

---

# 🔬 Lab 2 – `SIGSTOP` Se Process Stop Karein

Pehle ek background process start karein:

```bash
sleep 1000 &
```

Us ka PID dekhein:

```bash
jobs -l
```

Suppose PID hai:

```text
2500
```

Process stop karein:

```bash
kill -SIGSTOP 2500
```

Ya:

```bash
kill -19 2500
```

Verify karein:

```bash
jobs
```

Expected state:

```text
Stopped
```

`ps` ke saath bhi verify kar sakte hain:

```bash
ps -o pid,ppid,stat,cmd -p 2500
```

`STAT` column mein ye nazar aana chahiye:

```text
T
```

---

# `SIGTSTP` Aur `SIGSTOP` Mein Farq

| Feature | `SIGTSTP` | `SIGSTOP` |
|---------|-----------|-----------|
| Purpose | Process suspend karna | Forcefully suspend karna |
| Common Number | `20` | `19` |
| `Ctrl + Z` Se Bheja Jata Hai | Haan | Nahi |
| Handle Ho Sakta Hai | Haan | Nahi |
| Ignore Ho Sakta Hai | Haan | Nahi |
| Kernel-Enforced | Nahi | Haan |

---

# 6. `SIGCONT` – Stopped Process Ko Continue Karna

`SIGCONT` ka matlab hai:

> **Signal Continue**

Ye stopped ya suspended process ko dobara run karne ke liye use hota hai.

Common signal number:

```text
18
```

Jab process Stopped State mein ho aur aap usay dobara chalana chahte hon to `SIGCONT` use karein.

---

# Basic Syntax

Job ID ke saath:

```bash
kill -SIGCONT %1
```

PID ke saath:

```bash
kill -SIGCONT 2500
```

Signal number ke saath:

```bash
kill -18 2500
```

---

# 🔬 Lab 3 – `SIGCONT` Se Stopped Job Resume Karein

Current jobs check karein:

```bash
jobs -l
```

Example:

```text
[1]+ 2500 Stopped    sleep 1000
```

Resume karein:

```bash
kill -SIGCONT %1
```

Ya:

```bash
kill -18 %1
```

Verify karein:

```bash
jobs
```

Expected Output:

```text
[1]+  Running    sleep 1000 &
```

---

# `SIGCONT` Ke Baad Kya Hota Hai?

- Stopped process dobara Runnable State mein aata hai.
- Scheduler usay CPU time de sakta hai.
- Agar woh background job hai to background mein continue karega.
- Shell prompt available rahega.

---

# `SIGCONT` Flow

```text
Stopped Process
      │
      ▼
kill -SIGCONT PID
      │
      ▼
Process Runnable Hota Hai
      │
      ▼
Process Execution Continue Karta Hai
```

---

# 7. `SIGTERM` – Graceful Termination

`SIGTERM` ka matlab hai:

> **Signal Terminate**

Common signal number:

```text
15
```

Ye default signal hai jo `kill` command bina kisi signal option ke bhejti hai.

Ye commands equivalent hain:

```bash
kill PID
```

```bash
kill -SIGTERM PID
```

```bash
kill -15 PID
```

---

# `SIGTERM` Ko Pehle Kyun Use Karna Chahiye?

`SIGTERM` process ko mauqa deta hai ke woh:

- Data save kare.
- Open files close kare.
- Resources release kare.
- Temporary files remove kare.
- Clean shutdown perform kare.

Process `SIGTERM` ko catch ya handle kar sakta hai.

---

# 🔬 Lab 4 – `SIGTERM` Se Running Job Terminate Karein

Jobs display karein:

```bash
jobs -l
```

Suppose Job 2 ka PID hai:

```text
2502
```

PID ke zariye terminate karein:

```bash
kill -SIGTERM 2502
```

Ya:

```bash
kill -15 2502
```

Job ID ke zariye:

```bash
kill -SIGTERM %2
```

Verify karein:

```bash
jobs
```

Aap ko ye output nazar aa sakti hai:

```text
[2]+  Terminated    sleep 2000
```

---

# `SIGTERM` Flow

```text
Running Process
      │
      ▼
SIGTERM Bheja Jata Hai
      │
      ▼
Process Termination Request Receive Karta Hai
      │
      ├── Cleanup Perform Karta Hai
      │
      └── Gracefully Exit Karta Hai
```

---

# 8. `SIGKILL` – Forceful Termination

`SIGKILL` ka matlab hai:

> **Signal Kill**

Common signal number:

```text
9
```

Ye process ko foran terminate kar deta hai.

Command:

```bash
kill -SIGKILL PID
```

Ya:

```bash
kill -9 PID
```

---

# `SIGKILL` Ki Important Property

`SIGKILL` ko process:

- Catch nahi kar sakta.
- Handle nahi kar sakta.
- Block nahi kar sakta.
- Ignore nahi kar sakta.

Kernel process ko foran terminate kar deta hai.

Process ko cleanup ka mauqa nahi milta.

---

# 🔬 Lab 5 – Process Ko Forcefully Kill Karein

Suppose remaining process ka PID hai:

```text
2500
```

Kill karein:

```bash
kill -SIGKILL 2500
```

Ya:

```bash
kill -9 2500
```

Verify karein:

```bash
ps -p 2500
```

Koi process display nahi hona chahiye.

Shell jobs check karein:

```bash
jobs
```

Aap ko ye message mil sakta hai:

```text
[1]+  Killed    sleep 1000
```

---

# `SIGKILL` Kab Use Karna Chahiye?

`SIGKILL` sirf tab use karein jab:

- `SIGTERM` kaam na kare.
- Process unresponsive ho.
- Process stuck ho.
- Immediate termination zaroori ho.

Hamesha pehle `SIGTERM` try karein.

---

# Recommended Termination Procedure

```text
Step 1: SIGTERM Bhejein
        │
        ▼
Wait Aur Verify Karein
        │
        ├── Process Stop Ho Jaye ──► Finished
        │
        └── Process Abhi Bhi Running Ho
                     │
                     ▼
Step 2: SIGKILL Bhejein
```

Commands:

```bash
kill -15 PID
```

Check karein:

```bash
ps -p PID
```

Agar process abhi bhi mojood ho:

```bash
kill -9 PID
```

---

# `SIGTERM` Aur `SIGKILL` Mein Farq

| Feature | `SIGTERM` | `SIGKILL` |
|---------|-----------|-----------|
| Signal Number | `15` | `9` |
| Termination Type | Graceful Request | Immediate Force |
| Handle Ho Sakta Hai | Haan | Nahi |
| Ignore Ho Sakta Hai | Haan | Nahi |
| Cleanup Ka Mauqa | Haan | Nahi |
| Pehle Use Karna Chahiye | Haan | Nahi |
| Use Case | Normal termination | Unresponsive process |

---

# 9. Stop, Continue Aur Terminate Flow

```text
Running Process
      │
      ├── SIGTSTP ──► Stopped
      │
      ├── SIGSTOP ──► Stopped
      │                  │
      │                  └── SIGCONT ──► Running
      │
      ├── SIGTERM ──► Graceful Termination
      │
      └── SIGKILL ──► Immediate Termination
```

---

# 10. Job IDs Aur PIDs Ki Practice

Suppose:

```bash
jobs -l
```

ye output dikhata hai:

```text
[1]- 2500 Stopped    sleep 1000
[2]+ 2502 Running    sleep 2000 &
```

Aap dono forms use kar sakte hain.

## Job IDs Ke Saath

```bash
kill -SIGCONT %1
```

```bash
kill -SIGTERM %2
```

---

## PIDs Ke Saath

```bash
kill -SIGCONT 2500
```

```bash
kill -SIGTERM 2502
```

---

# 11. Process State Verify Karein

Command:

```bash
ps -o pid,ppid,stat,cmd -p PID
```

Example:

```bash
ps -o pid,ppid,stat,cmd -p 2500
```

Common states:

| `STAT` | Matlab |
|--------|--------|
| `R` | Running ya Runnable |
| `S` | Sleeping |
| `T` | Stopped |
| `Z` | Zombie |

---

# 12. Signal Information Dekhein

Tamam signals list karein:

```bash
kill -l
```

Signal number ko naam mein convert karein:

```bash
kill -l 15
```

Expected Output:

```text
TERM
```

Ek aur example:

```bash
kill -l 9
```

Expected Output:

```text
KILL
```

---

# 🧪 Practice Exercises

## Exercise 1 – Do Processes Start Karein

```bash
sleep 500 &
sleep 600 &
```

Jobs aur PIDs display karein:

```bash
jobs -l
```

---

## Exercise 2 – Job 1 Ko `SIGTSTP` Se Suspend Karein

```bash
kill -SIGTSTP %1
```

Verify karein:

```bash
jobs
```

---

## Exercise 3 – Job 1 Ko `SIGCONT` Se Resume Karein

```bash
kill -SIGCONT %1
```

Verify karein:

```bash
jobs
```

---

## Exercise 4 – Job 2 Ko `SIGSTOP` Se Stop Karein

```bash
kill -SIGSTOP %2
```

Verify karein:

```bash
jobs
```

---

## Exercise 5 – Job 2 Ko Continue Karein

```bash
kill -SIGCONT %2
```

---

## Exercise 6 – Job 1 Ko Gracefully Terminate Karein

```bash
kill -SIGTERM %1
```

---

## Exercise 7 – Job 2 Ko Forcefully Kill Karein

```bash
kill -SIGKILL %2
```

---

## Exercise 8 – Tamam Jobs Verify Karein

```bash
jobs
```

```bash
ps -ef | grep '[s]leep'
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – Process Stopped Hai

Check karein:

```bash
jobs -l
```

Resume karein:

```bash
kill -SIGCONT %1
```

Ya:

```bash
bg %1
```

---

### Scenario 2 – Process `Ctrl + Z` Se Stop Nahi Ho Raha

`SIGSTOP` bhejein:

```bash
kill -SIGSTOP PID
```

---

### Scenario 3 – Process Ko Cleanly Terminate Karna Hai

Use karein:

```bash
kill -SIGTERM PID
```

Wait aur verify karein:

```bash
ps -p PID
```

---

### Scenario 4 – Process `SIGTERM` Ignore Kar Raha Hai

Use karein:

```bash
kill -SIGKILL PID
```

Ye last resort hona chahiye.

---

### Scenario 5 – `kill %1` Par No Such Job Error

Job ID doosri shell se related ho sakta hai ya job already complete ho chuki hogi.

Check karein:

```bash
jobs
```

PID use karein:

```bash
ps aux | grep process_name
```

Phir:

```bash
kill -15 PID
```

---

### Scenario 6 – Signal Number Confirm Nahi Hai

Current system par check karein:

```bash
kill -l
```

Unusual architecture par signal numbers assume na karein.

---

# ⚠️ Important Safety Notes

- Signal bhejne se pehle PID hamesha verify karein.
- Jab tak bohot zaroori na ho `SIGKILL` pehle use na karein.
- Critical processes ko kill karne se ye problems ho sakti hain:
  - Data loss
  - Service interruption
  - Corrupted files
  - Users ka disconnect hona
  - System instability
- PID 1 ko kill karne se bachein.
- Production system par process terminate karne se pehle service identify karein.

`systemd` services ke liye process manually kill karne ke bajaye aam tor par ye use karna behtar hai:

```bash
systemctl stop service_name
```

---

# 📌 Quick Revision

| Signal | Number | Kaam | Handle Ho Sakta Hai? |
|--------|-------:|------|-----------------------|
| `SIGTSTP` | `20` | Terminal se suspend kare | Haan |
| `SIGSTOP` | `19` | Forcefully stop kare | Nahi |
| `SIGCONT` | `18` | Stopped process resume kare | Haan |
| `SIGTERM` | `15` | Graceful termination | Haan |
| `SIGKILL` | `9` | Immediate termination | Nahi |

---

# Common Commands

| Command | Kaam |
|---------|------|
| `kill -l` | Signals list kare |
| `kill -SIGTSTP PID` | Process suspend kare |
| `kill -SIGSTOP PID` | Process forcefully stop kare |
| `kill -SIGCONT PID` | Stopped process resume kare |
| `kill -SIGTERM PID` | Process gracefully terminate kare |
| `kill -15 PID` | `SIGTERM` ke barabar |
| `kill -SIGKILL PID` | Process forcefully terminate kare |
| `kill -9 PID` | `SIGKILL` ke barabar |
| `jobs -l` | Jobs aur PIDs display kare |
| `ps -p PID` | Process exist karta hai ya nahi verify kare |

---

# 📖 Key Takeaways

- Linux running processes ko control karne ke liye signals use karta hai.
- `SIGTSTP` process suspend karta hai aur application isay handle kar sakti hai.
- `SIGSTOP` process ko forcefully suspend karta hai aur ignore nahi ho sakta.
- `SIGCONT` stopped process ko resume karta hai.
- `SIGTERM` graceful termination request bhejta hai.
- `SIGKILL` process ko foran forcefully terminate karta hai.
- `SIGKILL` se pehle `SIGTERM` use karein.
- Signals naam ya number dono se bheje ja sakte hain.
- Job ID ke saath `%` lagta hai, PID ke saath nahi.
- Signal bhejne se pehle target process verify karein.

---

# 💡 Yaad Rakhein

> **Process Signals Ko Employee Ko Di Jane Wali Instructions Ki Tarah Samjhein.**
>
> - `SIGTSTP` – “Meherbani karke apna kaam temporary pause karo.”
> - `SIGSTOP` – “Foran kaam rok do.”
> - `SIGCONT` – “Apna kaam dobara continue karo.”
> - `SIGTERM` – “Apna kaam safely complete karke exit karo.”
> - `SIGKILL` – “Bina cleanup ke foran exit karo.”
>
> **Golden Signal Flow:**
>
> ```text
> Running
>    │
>    ├── SIGTSTP/SIGSTOP ──► Stopped
>    │                           │
>    │                           └── SIGCONT ──► Running
>    │
>    ├── SIGTERM ───────────► Graceful Exit
>    │
>    └── SIGKILL ───────────► Immediate Exit
> ```
>
> **Hamesha us signal ka istemal karein jo task complete karne ke liye sab se kam destructive ho.**