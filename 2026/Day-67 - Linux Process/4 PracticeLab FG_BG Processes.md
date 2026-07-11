# MODULE 08 – Practice Lab: Additional Foreground Aur Background Processes
> **Hands-on Practice Lab – `Ctrl + C`, `Ctrl + Z`, `fg`, `bg`, Aur `jobs` Se Multiple Jobs Control Karna (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `while` loop ki madad se ek continuous process create karna.
- Process ko foreground mein run karna.
- Samajhna ke foreground process terminal ko kaise occupy karta hai.
- `Ctrl + C` se process terminate karna.
- `Ctrl + Z` se process suspend karna.
- `jobs` command se shell jobs dekhna.
- `fg` se stopped job ko foreground mein resume karna.
- `bg` se stopped job ko background mein resume karna.
- Multiple stopped aur running jobs manage karna.
- Background job ko foreground mein lana.
- Samajhna ke multiple processes ek hi file mein write karein to kya hota hai.

---

# 📖 Introduction

Linux mein process do main tareeqon se run ho sakta hai:

1. **Foreground**
2. **Background**

Foreground process terminal ko occupy karta hai aur aam tor par keyboard input receive karta hai.

Background process terminal ko occupy kiye baghair run hota hai, jis se user doosri commands bhi execute kar sakta hai.

Is lab mein hum ek simple continuous loop banayenge aur usay use karke Linux Job Control practice karenge.

---

# 1. Continuous `while` Loop Create Karein

Neeche di gayi command har ek second baad ek file mein text write karti rahegi:

```bash
while true
do
    echo "Learning Processes" >> /tmp/process.log
    sleep 1
done
```

Isi loop ko ek hi line mein bhi likha ja sakta hai:

```bash
while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

---

# Loop Ko Samjhein

| Part | Matlab |
|------|--------|
| `while true` | Loop ko hamesha chalate raho |
| `do` | Loop ke andar commands start karo |
| `echo "Learning Processes"` | Message generate karo |
| `>> /tmp/process.log` | Message ko file ke end mein append karo |
| `sleep 1` | Ek second wait karo |
| `done` | Loop ka block complete karke dobara repeat karo |

---

# `>>` Kyun Use Kiya Gaya Hai?

Command mein use hua hai:

```text
>>
```

Ye new content ko file ke end mein **append** karta hai.

Agar aap use karein:

```text
>
```

to file har iteration par overwrite hogi aur aam tor par sirf latest line rahegi.

---

# 🔬 Lab 1 – Loop Ko Foreground Mein Run Karein

Run karein:

```bash
while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

Ab command continuously run karegi.

Observe karein:

- Terminal occupied hai.
- Shell prompt available nahi hai.
- Usi terminal mein doosri command type nahi ki ja sakti.
- Loop ko execute karne wala process foreground mein run kar raha hai.

---

# Foreground Process Flow

```text
User Loop Start Karta Hai
        │
        ▼
Shell Process Create Karta Hai
        │
        ▼
Process Foreground Mein Run Hota Hai
        │
        ▼
Terminal Occupied Ho Jata Hai
        │
        ▼
Loop Har Second File Mein Write Karta Hai
```

---

# 2. Doosre Terminal Se File Monitor Karein

Ek doosri terminal session open karein.

Run karein:

```bash
tail -f /tmp/process.log
```

Expected Output:

```text
Learning Processes
Learning Processes
Learning Processes
Learning Processes
```

Har ek second baad nayi line appear hogi.

---

# `tail -f` Ko Samjhein

| Part | Matlab |
|------|--------|
| `tail` | File ka last hissa display kare |
| `-f` | File ko follow kare jab new data add ho |

Ye prove karta hai ke foreground process continuously file update kar raha hai.

---

# 3. `Ctrl + C` Se Foreground Process Terminate Karein

Us terminal par wapas jayein jahan loop chal raha hai.

Press karein:

```text
Ctrl + C
```

Ye aam tor par interrupt signal bhejta hai:

```text
SIGINT
```

Process terminate ho jayega.

---

# `Ctrl + C` Ke Baad Kya Hota Hai?

- Loop stop ho jata hai.
- Process terminate ho jata hai.
- Shell prompt wapas aa jata hai.
- `/tmp/process.log` update hona band ho jati hai.

Doosre terminal mein verify karein:

```bash
tail -f /tmp/process.log
```

Output update hona band ho jana chahiye.

---

# `Ctrl + C` Flow

```text
Foreground Process Running
        │
        ▼
User Ctrl + C Press Karta Hai
        │
        ▼
SIGINT Send Hota Hai
        │
        ▼
Process Terminate Ho Jata Hai
        │
        ▼
Shell Prompt Wapas Aata Hai
```

---

# 4. Loop Dobarah Start Karein

Dobarah run karein:

```bash
while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

Ek naya process create hoga.

File dobara update hona start ho jayegi.

---

# 5. `Ctrl + Z` Se Process Suspend Karein

Terminate karne ke bajaye press karein:

```text
Ctrl + Z
```

Expected Output:

```text
[1]+  Stopped    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

Process ab suspend ho chuka hai.

---

# `Ctrl + Z` Aur `Ctrl + C` Mein Farq

| Shortcut | Result |
|----------|--------|
| `Ctrl + C` | Process terminate karta hai |
| `Ctrl + Z` | Process ko pause ya suspend karta hai |

Suspended process abhi exist karta hai, lekin actively execute nahi karta.

---

# 6. Stopped Job Dekhein

Run karein:

```bash
jobs
```

Example Output:

```text
[1]+  Stopped    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

---

# `jobs` Output Ko Samjhein

| Field | Matlab |
|-------|--------|
| `[1]` | Job Number |
| `+` | Current ya default job |
| `Stopped` | Job state |
| Command | Job se related shell command |

---

# 7. Job Ko Foreground Mein Resume Karein

Run karein:

```bash
fg %1
```

Process foreground mein resume ho jayega.

Observe karein:

- Terminal dobara occupied hai.
- Shell prompt disappear ho gaya.
- `/tmp/process.log` dobara update hona start ho gayi.

---

# `fg %1` Ko Samjhein

| Part | Matlab |
|------|--------|
| `fg` | Job ko foreground mein move ya resume kare |
| `%1` | Shell Job Number 1 |

---

# 8. Terminal Disconnect Aur Foreground Process

Foreground process terminal session ke saath associated hota hai.

Agar terminal close ya disconnect ho jaye to process ko ye signal mil sakta hai:

```text
SIGHUP
```

Process terminate ho sakta hai, jab tak usay in tools se protect na kiya gaya ho:

- `nohup`
- `screen`
- `tmux`
- `systemd`

---

# 9. Job Ko Dobarah Suspend Karein

Jab loop foreground mein run ho raha ho, press karein:

```text
Ctrl + Z
```

Job dobara Stopped state mein chali jayegi.

Check karein:

```bash
jobs
```

---

# 10. Doosra Foreground Process Start Karein

Wahi loop dobara run karein:

```bash
while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

Phir press karein:

```text
Ctrl + Z
```

Ab check karein:

```bash
jobs
```

Example Output:

```text
[1]-  Stopped    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
[2]+  Stopped    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
```

Ab aap ke paas do stopped jobs hain.

---

# `+` Aur `-` Ka Matlab

| Symbol | Matlab |
|--------|--------|
| `+` | Current ya default job |
| `-` | Previous job |

Agar `fg` ya `bg` bina job number ke chalaya jaye to aam tor par `+` wali job par apply hota hai.

---

# 11. Job Ko Background Mein Resume Karein

Job 2 ko background mein resume karne ke liye:

```bash
bg %2
```

Example Output:

```text
[2]+ while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done &
```

Aakhir mein ampersand dekhein:

```text
&
```

Ye batata hai ke process background mein run kar raha hai.

---

# Background Mein Kya Hota Hai?

- Loop resume ho jata hai.
- File dobara update hoti hai.
- Terminal available rehta hai.
- Aap doosri commands chala sakte hain.

Check karein:

```bash
jobs
```

Example:

```text
[1]+  Stopped    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
[2]-  Running    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done &
```

---

# 12. Doosri Job Ko Foreground Mein Resume Karein

Suppose Job 1 stopped hai.

Run karein:

```bash
fg %1
```

Ab:

- Job 1 foreground mein run karegi.
- Job 2 background mein continue karegi.
- Dono processes same file mein write karenge.

---

# Do Processes Ek Hi File Mein Write Kar Rahe Hain

```text
Job 1 – Foreground
        │
        ├── "Learning Processes" Write Karti Hai
        │
Job 2 – Background
        │
        └── "Learning Processes" Write Karti Hai
                    │
                    ▼
          /tmp/process.log
```

File zyada tezi se update ho sakti hai kyun ke do processes us mein write kar rahe hain.

---

# 13. Foreground Job Ko Dobarah Suspend Karein

Press karein:

```text
Ctrl + Z
```

Ab sab jobs check karein:

```bash
jobs
```

Ek job background mein running ho sakti hai aur doosri stopped.

---

# 14. Different Text Ke Saath Teesra Loop Start Karein

Run karein:

```bash
while true; do echo "This is my third script" >> /tmp/process.log; sleep 1; done
```

Ab file mein different messages aa sakte hain.

Example:

```text
Learning Processes
This is my third script
Learning Processes
This is my third script
```

Teesri job suspend karne ke liye press karein:

```text
Ctrl + Z
```

---

# 15. Tamam Jobs Dekhein

Run karein:

```bash
jobs
```

Example:

```text
[1]   Stopped    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done
[2]-  Running    while true; do echo "Learning Processes" >> /tmp/process.log; sleep 1; done &
[3]+  Stopped    while true; do echo "This is my third script" >> /tmp/process.log; sleep 1; done
```

---

# 16. Background Process Ko Foreground Mein Laayein

Suppose Job 2 background mein run kar rahi hai.

Run karein:

```bash
fg %2
```

Process background se foreground mein aa jayega.

Ab:

- Terminal occupied ho jayega.
- Process terminal input receive kar sakta hai.
- Baqi stopped jobs stopped hi rahengi.

Dobarah suspend karne ke liye press karein:

```text
Ctrl + Z
```

---

# Background Se Foreground Flow

```text
Background Job
      │
      ▼
fg %2
      │
      ▼
Foreground Job
      │
      ▼
Terminal Occupied
```

---

# 17. Important Job-Control Commands

| Command | Kaam |
|---------|------|
| `jobs` | Current shell ke jobs list kare |
| `fg` | Current job ko foreground mein resume kare |
| `fg %1` | Job 1 ko foreground mein resume kare |
| `bg` | Current stopped job ko background mein resume kare |
| `bg %1` | Job 1 ko background mein resume kare |
| `kill %1` | Job 1 ko termination signal bheje |
| `jobs -l` | Jobs ko un ke PIDs ke saath display kare |

---

# 18. Job Number Aur PID Mein Farq

Job number aur PID same nahi hote.

| Type | Example | Kis Ke Zariye Manage Hota Hai |
|------|---------|--------------------------------|
| Job Number | `%1` | Current shell |
| PID | `4205` | Linux Kernel |

Dono dekhne ke liye:

```bash
jobs -l
```

Example:

```text
[1]+ 4205 Running    sleep 300 &
```

---

# 19. Tamam Practice Jobs Stop Karein

Pehle jobs display karein:

```bash
jobs
```

Individual jobs terminate karein:

```bash
kill %1
kill %2
kill %3
```

Verify karein:

```bash
jobs
```

Latest completion status dekhne ke liye kabhi kabhi dobara **Enter** press karna padta hai.

---

# Alternative: Current Shell Ki Tamam Jobs Kill Karein

Common command:

```bash
kill $(jobs -p)
```

Ye current shell ke tamam jobs ke PIDs ko default termination signal bhejta hai.

Is command ko ehtiyaat se use karein.

---

# 20. Practice File Clean Karein

Lab complete hone ke baad test file remove karein:

```bash
rm -f /tmp/process.log
```

Verify karein:

```bash
ls -l /tmp/process.log
```

Expected Output:

```text
ls: cannot access '/tmp/process.log': No such file or directory
```

---

# 21. Complete Job-Control Workflow

```text
Process Start Karein
      │
      ▼
Foreground
      │
      ├── Ctrl + C ─────────► Terminated
      │
      └── Ctrl + Z ─────────► Stopped
                                  │
                                  ├── fg %job ─► Foreground
                                  │
                                  ├── bg %job ─► Background
                                  │
                                  └── kill %job ─► Terminated

Background
      │
      ├── fg %job ──────────► Foreground
      ├── kill %job ────────► Terminated
      └── Terminal Available Rehte Hue Continue Karta Hai
```

---

# 🧪 Practice Exercises

## Exercise 1 – Foreground Loop Create Karein

```bash
while true; do echo "Process One" >> /tmp/jobs.log; sleep 1; done
```

Observe karein ke terminal occupied ho gaya.

---

## Exercise 2 – File Monitor Karein

Doosre terminal se:

```bash
tail -f /tmp/jobs.log
```

---

## Exercise 3 – Loop Terminate Karein

Press karein:

```text
Ctrl + C
```

Verify karein ke file update hona band ho gaya.

---

## Exercise 4 – Loop Suspend Karein

Loop dobara start karein aur press karein:

```text
Ctrl + Z
```

Check karein:

```bash
jobs
```

---

## Exercise 5 – Foreground Mein Resume Karein

```bash
fg %1
```

---

## Exercise 6 – Suspend Karke Background Mein Resume Karein

Press karein:

```text
Ctrl + Z
```

Phir:

```bash
bg %1
```

---

## Exercise 7 – Doosri Job Start Karein

```bash
while true; do echo "Process Two" >> /tmp/jobs.log; sleep 2; done
```

Press karein:

```text
Ctrl + Z
```

Check karein:

```bash
jobs
```

---

## Exercise 8 – Dono Jobs Run Karein

Ek ko background mein resume karein:

```bash
bg %1
```

Doosri ko foreground mein resume karein:

```bash
fg %2
```

---

## Exercise 9 – Job PIDs Dekhein

```bash
jobs -l
```

---

## Exercise 10 – Tamam Jobs Terminate Karein

```bash
kill $(jobs -p)
```

Verify karein:

```bash
jobs
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – Terminal Occupied Hai

Long process foreground mein run kar raha hai.

Press karein:

```text
Ctrl + Z
```

Phir background mein resume karein:

```bash
bg
```

---

### Scenario 2 – Process Ghalti Se Suspend Ho Gaya

Check karein:

```bash
jobs
```

Foreground mein resume karein:

```bash
fg %1
```

Ya background mein:

```bash
bg %1
```

---

### Scenario 3 – File Update Nahi Ho Rahi

Job status check karein:

```bash
jobs
```

Agar output ho:

```text
Stopped
```

to resume karein:

```bash
bg %job_number
```

---

### Scenario 4 – Multiple Jobs Same File Update Kar Rahi Hain

Jobs list karein:

```bash
jobs -l
```

Ghair zaroori job terminate karein:

```bash
kill %job_number
```

---

### Scenario 5 – `fg %1` Par No Such Job Error

Job shayad complete ho chuki hai, terminate ho gayi hai, ya kisi doosri shell session se related hai.

Check karein:

```bash
jobs
```

Yaad rakhein ke shell job numbers usi terminal session ke andar valid hote hain jahan job start hui thi.

---

### Scenario 6 – Process Logout Ke Baad Bhi Continue Karna Chahiye

Normal shell background process logout ke baad terminate ho sakta hai.

Use karein:

```bash
nohup command > output.log 2>&1 &
```

Ya:

```text
screen
tmux
systemd
```

---

# 📌 Quick Revision

| Action | Command Ya Shortcut |
|--------|----------------------|
| Foreground mein run kare | `command` |
| Direct background mein run kare | `command &` |
| Foreground process terminate kare | `Ctrl + C` |
| Foreground process suspend kare | `Ctrl + Z` |
| Jobs list kare | `jobs` |
| Jobs aur PIDs dekhein | `jobs -l` |
| Job 1 ko foreground mein resume kare | `fg %1` |
| Job 1 ko background mein resume kare | `bg %1` |
| Job 1 terminate kare | `kill %1` |
| Current shell ki tamam jobs terminate kare | `kill $(jobs -p)` |
| File monitor kare | `tail -f file` |

---

# 📖 Key Takeaways

- Foreground process terminal ko occupy karta hai.
- `Ctrl + C` foreground process terminate karta hai.
- `Ctrl + Z` foreground process ko terminate kiye baghair suspend karta hai.
- `jobs` current shell se related jobs dikhata hai.
- `fg` job ko foreground mein move ya resume karta hai.
- `bg` stopped job ko background mein resume karta hai.
- Background jobs terminal ko available rakhti hain.
- Ek hi waqt mein multiple jobs run ho sakti hain.
- Multiple processes ek hi file mein write kar sakte hain, jis se file zyada tezi se update ho sakti hai.
- `%1` jaisi job numbers current shell ke zariye manage hoti hain.
- PIDs Linux Kernel assign aur manage karta hai.

---

# 💡 Yaad Rakhein

> **Foreground Aur Background Jobs Ko Shared Desk Par Kaam Karne Wale Workers Ki Tarah Samjhein.**
>
> - **Foreground Job** aap ke desk par baithti hai, is liye aap desk use nahi kar sakte jab tak woh complete na ho.
> - `Ctrl + Z` worker ko temporary pause karta hai.
> - `bg` worker ko background mein kaam continue karne bhejta hai.
> - `fg` worker ko dobara aap ke desk par le aata hai.
> - `Ctrl + C` worker ka task khatam kar deta hai.
>
> **Golden Job-Control Flow:**
>
> ```text
> Foreground
>     │
>     ├── Ctrl + C ──► Terminated
>     │
>     └── Ctrl + Z ──► Stopped
>                         │
>                         ├── bg ──► Background
>                         └── fg ──► Foreground
> ```