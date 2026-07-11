# MODULE 08 – Additional Notes: Process Control with Signals
> **Linux Signals Ki Madad Se Running Processes Ko Control Karna (Roman Urdu)**

---

# 🎯 Learning Objectives

In notes mein aap seekhenge:

- Linux Signal kya hota hai.
- Signals ko Software Interrupt kyun kaha jata hai.
- Signals running processes par kis tarah asar daalte hain.
- Available Linux signals ko kaise list karna hai.
- `Ctrl + Z` process ko Running se Stopped state mein kaise le jata hai.
- `SIGTSTP` kis tarah kaam karta hai.
- `SIGSTOP`, `SIGTSTP` se kis tarah mukhtalif hai.
- Job ID ya Process ID use karke signals kaise bhejne hain.
- Process state change ko kaise verify karna hai.

---

# 📖 Introduction

Signals ke zariye Process Control Linux administration ka bohot important topic hai.

Linux process ko zarurat ke mutabiq:

- Interrupt
- Suspend
- Resume
- Terminate
- Forcefully stop
- Kisi event ke bare mein notify

kiya ja sakta hai.

Linux ye tamam kaam **Signals** ki madad se karta hai.

---

# 1. Signal Kya Hai?

Signal hota hai:

> **Ek Software Interrupt jo running process ko deliver kiya jata hai.**

Signal process ko batata hai ke koi specific event hua hai, ya process se koi particular action perform karne ki request karta hai.

Misal ke taur par signal process se keh sakta hai:

- Temporary stop ho jao.
- Dobarah continue karo.
- Gracefully terminate ho jao.
- Foran terminate ho jao.
- Configuration reload karo.
- Terminal event ka response do.

---

# 2. Signals Important Kyun Hain?

Suppose system par koi process run kar raha hai.

User ya Administrator ko zarurat ho sakti hai ke woh:

- Process ko interrupt kare.
- Us ki state Running se Stopped mein change kare.
- Stopped process ko resume kare.
- Process terminate kare.
- Agar process response na de to forcefully kill kare.

Signals processes ko program code directly modify kiye baghair control karne ka standard tareeqa provide karte hain.

---

# 3. Kaun Se Events Signals Generate Karte Hain?

Signals mukhtalif sources se generate ho sakte hain:

- Keyboard shortcuts
- Errors
- Hardware events
- Terminal events
- Linux Kernel
- Koi doosra process
- `kill` jaisi command
- Shell Job-Control command

Examples:

| Event | Signal |
|------|--------|
| `Ctrl + C` press karna | `SIGINT` |
| `Ctrl + Z` press karna | `SIGTSTP` |
| Terminal close hona | `SIGHUP` |
| `kill PID` | Default tor par `SIGTERM` |
| `kill -9 PID` | `SIGKILL` |

---

# 4. Tamam Available Signals List Karein

Command:

```bash
kill -l
```

Ye command available signals ke names aur numbers display karti hai.

Example:

```text
1) SIGHUP
2) SIGINT
9) SIGKILL
15) SIGTERM
18) SIGCONT
19) SIGSTOP
20) SIGTSTP
```

Kuch architectures par signal numbers mukhtalif ho sakte hain, is liye current system par verify karne ke liye `kill -l` use karein.

---

# 5. Long-Running Foreground Process Create Karein

`sleep` command ke saath koi bara number use karein.

```bash
sleep 1000
```

Ye process 1000 seconds tak active rahega.

Kyun ke ampersand (`&`) use nahi hua:

- Process foreground mein run karega.
- Terminal occupied rahega.
- Shell prompt available nahi hoga.
- Process terminal se keyboard input receive kar sakta hai.

---

# Foreground Process Flow

```text
User sleep 1000 Run Karta Hai
        │
        ▼
Shell Process Start Karta Hai
        │
        ▼
Process Foreground Mein Run Hota Hai
        │
        ▼
Terminal Occupied Rehta Hai
```

---

# 6. `Ctrl + Z` Se Foreground Process Stop Karein

Jab command run ho rahi ho to press karein:

```text
Ctrl + Z
```

Expected Output:

```text
[1]+  Stopped    sleep 1000
```

Process suspend ho chuka hai.

Ye terminate nahi hua.

---

# Background Mein Kya Hota Hai?

Jab aap press karte hain:

```text
Ctrl + Z
```

to terminal aam tor par foreground process group ko ye signal bhejta hai:

```text
SIGTSTP
```

Standard x86_64 Linux systems par iska common signal number hai:

```text
20
```

---

# 7. Stopped Process Verify Karein

Run karein:

```bash
jobs
```

Expected Output:

```text
[1]+  Stopped    sleep 1000
```

Zyada detail ke liye:

```bash
jobs -l
```

Example:

```text
[1]+ 2500 Stopped    sleep 1000
```

Yahan:

- `[1]` Job ID hai.
- `2500` Process ID hai.
- `Stopped` job ki current state hai.

---

# 8. `SIGTSTP` Kya Hai?

`SIGTSTP` ka matlab hai:

> **Signal Terminal Stop**

Ye process se us ki execution stop ya suspend karne ki request karta hai.

Common signal number:

```text
20
```

Ye aam tor par is shortcut se generate hota hai:

```text
Ctrl + Z
```

---

# `SIGTSTP` Ki Important Property

Process `SIGTSTP` ko:

- Catch kar sakta hai.
- Handle kar sakta hai.
- Ignore kar sakta hai.
- Block kar sakta hai.

Is liye application decide kar sakti hai ke is signal ka kya response dena hai.

---

# 9. Background Process Start Karein

Run karein:

```bash
sleep 2000 &
```

Example Output:

```text
[2] 2502
```

Ampersand (`&`) process ko background mein run karwata hai.

Shell prompt foran wapas aa jata hai.

---

# Background Job Verify Karein

Run karein:

```bash
jobs -l
```

Example:

```text
[1]- 2500 Stopped    sleep 1000
[2]+ 2502 Running    sleep 2000 &
```

---

# 10. `SIGTSTP` Se Background Process Stop Karein

Aap `SIGTSTP` do tareeqon se bhej sakte hain:

- Job ID
- Process ID

---

# Job ID Ke Saath

```bash
kill -SIGTSTP %2
```

`%` symbol shell ko batata hai ke `2` ek Job ID hai.

---

# PID Ke Saath

Suppose PID hai:

```text
2502
```

Run karein:

```bash
kill -SIGTSTP 2502
```

Signal number ke saath bhi use kar sakte hain:

```bash
kill -20 2502
```

---

# State Change Verify Karein

Run karein:

```bash
jobs
```

Expected Output:

```text
[1]-  Stopped    sleep 1000
[2]+  Stopped    sleep 2000
```

Doosre process ki state change hui:

```text
Running
```

se:

```text
Stopped
```

---

# 11. `ps` Se Process ID Dhoondein

`sleep` naam ke processes dhoondhne ke liye:

```bash
ps -C sleep
```

Example Output:

```text
PID TTY          TIME CMD
2500 pts/0    00:00:00 sleep
2502 pts/0    00:00:00 sleep
```

Zyada detailed output ke liye:

```bash
ps -C sleep -o pid,ppid,stat,cmd
```

Example:

```text
PID   PPID STAT CMD
2500  2400 T    sleep 1000
2502  2400 T    sleep 2000
```

`STAT` column mein:

```text
T
```

ka matlab hai:

```text
Stopped
```

---

# 12. Job ID Aur Process ID Mein Farq

| Identifier | Example | Kis Ke Zariye Manage Hota Hai |
|------------|---------|--------------------------------|
| Job ID | `%2` | Current Shell |
| PID | `2502` | Linux Kernel |

Job IDs sirf us shell mein valid hote hain jahan job start hui ho.

PIDs system-wide process ko identify karte hain.

---

# 13. Signal Naam Ya Number Se Bhejein

Dono forms valid hain.

Signal name ke saath:

```bash
kill -SIGTSTP 2502
```

Signal number ke saath:

```bash
kill -20 2502
```

Job ID ke saath:

```bash
kill -SIGTSTP %2
```

---

# 14. `SIGSTOP` Kya Hai?

`SIGSTOP` bhi process ko stop ya suspend karta hai.

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

`SIGTSTP` ke baraks, `SIGSTOP` ko process:

- Catch nahi kar sakta.
- Handle nahi kar sakta.
- Block nahi kar sakta.
- Ignore nahi kar sakta.

Linux Kernel process ko forcefully Stopped state mein bhej deta hai.

---

# 15. `SIGTSTP` Aur `SIGSTOP` Mein Farq

| Feature | `SIGTSTP` | `SIGSTOP` |
|---------|-----------|-----------|
| Matlab | Terminal Stop Request | Forceful Stop |
| Common Number | `20` | `19` |
| `Ctrl + Z` Se Bheja Jata Hai | Haan | Nahi |
| Catch Ho Sakta Hai | Haan | Nahi |
| Ignore Ho Sakta Hai | Haan | Nahi |
| Block Ho Sakta Hai | Haan | Nahi |
| Kernel-Enforced | Puri tarah nahi | Haan |

---

# 16. `SIGSTOP` Kab Use Karna Chahiye?

`SIGSTOP` tab use karein jab:

- Process `SIGTSTP` ignore kare.
- Process ko foran suspend karna zaroori ho.
- Kernel-enforced stop required ho.
- Normal job-control suspension kaam na kare.

Example:

```bash
kill -SIGSTOP 2502
```

---

# 17. `ps` Se Process State Verify Karein

Run karein:

```bash
ps -o pid,ppid,stat,cmd -p 2502
```

Example:

```text
PID   PPID STAT CMD
2502  2400 T    sleep 2000
```

Process state:

```text
T
```

ka matlab hai ke process stopped hai.

---

# 18. Process State Flow

```text
Running Process
      │
      ├── Ctrl + Z
      │      │
      │      ▼
      │   SIGTSTP
      │      │
      │      ▼
      │   Stopped
      │
      └── SIGSTOP
             │
             ▼
          Stopped
```

---

# 19. Stop Ka Matlab Terminate Nahi Hota

Stopped process:

- Abhi bhi exist karta hai.
- Apna PID retain karta hai.
- Memory mein mojood rehta hai.
- Normal execution continue nahi karta.
- Baad mein resume kiya ja sakta hai.
- `jobs`, `ps`, aur `top` mein nazar aata hai.

Terminated process:

- Apna kaam finish karta hai ya kill ho jata hai.
- Zyada tar resources release kar deta hai.
- Aakhir mein process listings se disappear ho jata hai.

---

# 20. Practice Lab Summary

Basic workflow:

```bash
sleep 1000
```

Press karein:

```text
Ctrl + Z
```

Check karein:

```bash
jobs -l
```

Doosra process start karein:

```bash
sleep 2000 &
```

PID dhoondein:

```bash
ps -C sleep
```

PID se stop karein:

```bash
kill -SIGTSTP PID
```

Ya forcefully stop karein:

```bash
kill -SIGSTOP PID
```

Verify karein:

```bash
jobs
```

```bash
ps -C sleep -o pid,ppid,stat,cmd
```

---

# 🧪 Practice Exercises

## Exercise 1 – Signals List Karein

```bash
kill -l
```

In signals ko identify karein:

- `SIGTSTP`
- `SIGSTOP`
- `SIGCONT`
- `SIGTERM`
- `SIGKILL`

---

## Exercise 2 – Foreground Process Start Karein

```bash
sleep 500
```

---

## Exercise 3 – Process Suspend Karein

Press karein:

```text
Ctrl + Z
```

Verify karein:

```bash
jobs -l
```

---

## Exercise 4 – Background Process Start Karein

```bash
sleep 600 &
```

Verify karein:

```bash
jobs -l
```

---

## Exercise 5 – PID Dhoondein

```bash
ps -C sleep
```

---

## Exercise 6 – `SIGTSTP` Se Stop Karein

```bash
kill -SIGTSTP PID
```

Verify karein:

```bash
jobs
```

---

## Exercise 7 – Doosre Process Ko `SIGSTOP` Se Stop Karein

Start karein:

```bash
sleep 700 &
```

PID dekhein:

```bash
jobs -l
```

Stop karein:

```bash
kill -SIGSTOP PID
```

Verify karein:

```bash
ps -o pid,stat,cmd -p PID
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – `Ctrl + Z` Process Ko Stop Nahi Karta

Process shayad `SIGTSTP` handle ya ignore kar raha hai.

Use karein:

```bash
kill -SIGSTOP PID
```

---

### Scenario 2 – `kill -SIGTSTP %2` Par No Such Job Error

Job:

- Doosri shell se related ho sakti hai.
- Already complete ho sakti hai.
- Us ka Job ID mukhtalif ho sakta hai.

Check karein:

```bash
jobs -l
```

Ya PID use karein:

```bash
ps -C process_name
```

---

### Scenario 3 – Multiple `sleep` Processes Mojood Hain

Use karein:

```bash
ps -C sleep -o pid,ppid,stat,cmd
```

Signal bhejne se pehle correct PID confirm karein.

---

### Scenario 4 – Process State Change Nahi Ho Rahi

Verify karein:

- Aap ke paas process ko signal bhejne ki permission hai.
- PID correct hai.
- Process abhi bhi exist karta hai.
- Aap ne correct signal use kiya hai.

Check karein:

```bash
ps -p PID
```

---

# ⚠️ Important Safety Notes

- Signal bhejne se pehle PID hamesha verify karein.
- Critical system processes ko un ka purpose samjhe baghair stop na karein.
- Service process stop karne se service unavailable ho sakti hai.
- Job IDs sirf current shell mein valid hote hain.
- Root aam tor par doosre users ke processes ko bhi signal bhej sakta hai.
- Normal user aam tor par sirf apne owned processes ko signal bhej sakta hai.

---

# 📌 Quick Revision

| Item | Matlab |
|------|--------|
| Signal | Process ko deliver hone wala Software Interrupt |
| `kill -l` | Signal names aur numbers list kare |
| `Ctrl + Z` | Foreground process group ko `SIGTSTP` bheje |
| `SIGTSTP` | Process se stop hone ki request |
| `SIGSTOP` | Process ko forcefully stop kare |
| `%2` | Shell Job ID 2 |
| `2502` | Linux Process ID |
| `jobs -l` | Shell jobs aur un ke PIDs show kare |
| `ps -C sleep` | Command name ke zariye process dhoonde |
| `T` state | Stopped Process |

---

# Common Commands

| Command | Kaam |
|---------|------|
| `kill -l` | Available signals list kare |
| `sleep 1000` | Foreground test process create kare |
| `sleep 2000 &` | Background test process create kare |
| `jobs` | Current shell jobs display kare |
| `jobs -l` | Jobs ko PIDs ke saath display kare |
| `ps -C sleep` | Tamam `sleep` processes dhoonde |
| `kill -SIGTSTP PID` | Process se stop hone ki request kare |
| `kill -20 PID` | Number se `SIGTSTP` bheje |
| `kill -SIGSTOP PID` | Process ko forcefully stop kare |
| `kill -19 PID` | Number se `SIGSTOP` bheje |
| `ps -o pid,ppid,stat,cmd -p PID` | Process state verify kare |

---

# 📖 Key Takeaways

- Signal ek Software Interrupt hai jo process ko deliver hota hai.
- Signals running processes ko control aur notify karne ke liye use hote hain.
- `kill -l` available signal names aur numbers list karta hai.
- `Ctrl + Z` aam tor par `SIGTSTP` bhejta hai.
- `SIGTSTP` process ko Running se Stopped state mein le jata hai.
- `SIGSTOP` bhi process ko stop karta hai, lekin isay catch ya ignore nahi kiya ja sakta.
- Signals Job IDs ya PIDs dono ke zariye bheje ja sakte hain.
- `T` process state ka matlab Stopped hai.
- Process ko stop karna aur terminate karna alag cheezen hain.
- Signal bhejne se pehle hamesha correct target verify karein.

---

# 💡 Yaad Rakhein

> **Signal Ko Worker Ko Bheji Jane Wali Instruction Ki Tarah Samjhein.**
>
> - `SIGTSTP` ka matlab: **“Meherbani karke apna kaam pause karo.”**
> - `SIGSTOP` ka matlab: **“Chahe tum chaho ya na chaho, foran stop ho jao.”**
>
> **Golden Flow:**
>
> ```text
> Running Process
>       │
>       ├── Ctrl + Z / SIGTSTP
>       │              │
>       │              ▼
>       │           Stopped
>       │
>       └── SIGSTOP
>              │
>              ▼
>           Stopped
> ```
>
> **Normal suspension ke liye `SIGTSTP` use karein, aur jab process ko forcefully stop karna ho to `SIGSTOP` use karein.**