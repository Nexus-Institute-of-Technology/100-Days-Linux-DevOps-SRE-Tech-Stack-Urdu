# MODULE 08 – Practice Lab: Control Jobs
> **Hands-on Practice Lab – Foreground Aur Background Processes (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- Linux Job Control ko samajhna.
- Foreground Processes ko identify karna.
- Background Processes ko identify karna.
- Commands ko foreground mein run karna.
- Commands ko background mein run karna.
- Terminal aur running processes ke darmiyan interaction ko samajhna.
- Ampersand (`&`) ki madad se background job start karna.
- Ek process ke running hone ke dauran terminal par doosri commands chalana.

---

# 📖 Introduction

Jab aap Linux mein koi command ya program start karte hain to woh ek process ki shakal mein run hota hai.

Process do main tareeqon se run ho sakta hai:

1. **Foreground Process**
2. **Background Process**

Default tor par commands foreground mein run hoti hain.

Foreground process terminal ko tab tak occupy karta hai jab tak woh complete na ho jaye.

Background process terminal ko occupy kiye baghair run hota hai, jis ki wajah se aap doosri commands bhi execute kar sakte hain.

---

# 1. Job Control Kya Hai?

**Job Control** Linux shell ka ek feature hai jo current terminal se start hone wale processes ko manage karne ki sahulat deta hai.

Job Control ki madad se aap:

- Command ko foreground mein run kar sakte hain.
- Command ko background mein run kar sakte hain.
- Running process ko suspend kar sakte hain.
- Suspended process ko resume kar sakte hain.
- Jobs ko foreground aur background ke darmiyan move kar sakte hain.
- Current shell ke active jobs dekh sakte hain.

---

# Process Aur Job Mein Farq

**Process** kisi running program ki instance hota hai.

**Job** current shell se start ki gayi command ya command pipeline hoti hai.

Ek job ke andar ek ya zyada processes ho sakte hain.

Example:

```bash
cat file1 | grep error
```

Ye ek shell job hai, lekin is ke andar multiple processes hain.

---

# 2. Foreground Process

**Foreground Process** directly current terminal ke andar run hota hai.

Default tor par har command foreground mein start hoti hai.

Examples:

```bash
pwd
```

```bash
ls
```

```bash
./backup.sh
```

Jab Foreground Process run ho raha ho:

- Woh keyboard se input receive karta hai.
- Output screen par bhejta hai.
- Terminal ko occupy karta hai.
- Shell prompt available nahi hota.
- Aam tor par aap usi terminal mein doosri command tab tak nahi chala sakte jab tak process complete na ho.

---

# 🔬 Lab 1 – Foreground Command Run Karein

Command chalayein:

```bash
pwd
```

Ye command foreground mein run hogi.

Expected Output:

```text
/home/dev1
```

Process jaldi complete ho jata hai aur shell prompt wapas aa jata hai.

---

# 🔬 Lab 2 – `ls` Ko Foreground Mein Run Karein

Command:

```bash
ls
```

Background mein kya hota hai?

1. Bash command receive karta hai.
2. Bash `ls` process start karta hai.
3. Process foreground mein run hota hai.
4. Output screen par display hoti hai.
5. Process exit karta hai.
6. Shell prompt wapas aa jata hai.

---

# Foreground Process Flow

```text
User Command Enter Karta Hai
        │
        ▼
Shell Process Start Karta Hai
        │
        ▼
Process Foreground Mein Run Hota Hai
        │
        ▼
Terminal Occupied Rehta Hai
        │
        ▼
Process Complete Hota Hai
        │
        ▼
Shell Prompt Wapas Aata Hai
```

---

# 3. Foreground Process Aur Terminal Input

Foreground Process terminal ke saath connected hota hai.

Ye:

- Keyboard input read kar sakta hai.
- Screen par output display kar sakta hai.
- Terminal-generated signals receive kar sakta hai.

Example:

```bash
cat
```

`cat` command aap ke input ka wait karti hai.

Command finish karne ke liye press karein:

```text
Ctrl + D
```

Ye End-of-File signal bhejta hai.

---

# 4. Long-Running Foreground Jobs Ka Masla

Suppose aap ek long backup script run karte hain:

```bash
./backup.sh
```

Agar script ko 30 minutes lagte hain:

- Terminal occupied rahega.
- Prompt available nahi hoga.
- Aap us terminal mein doosri command nahi chala sakenge.
- Aap ko script complete hone ka wait karna padega.

Short commands ke liye ye theek hai, lekin long-running jobs ke liye inconvenient ho sakta hai.

---

# 5. Background Process

**Background Process** terminal ko occupy kiye baghair run hota hai.

Ye aam tor par terminal se direct keyboard input receive nahi karta.

Shell prompt foran wapas aa jata hai, jis se aap doosri commands run kar sakte hain.

Background Process useful hota hai:

- Backups
- Long scripts
- File transfers
- Compression jobs
- Data processing
- Long-running maintenance tasks

---

# 6. `&` Ke Saath Background Process Start Karna

Command ko background mein run karne ke liye us ke end par ampersand (`&`) lagayein.

Syntax:

```bash
command &
```

Example:

```bash
ls &
```

Shell command ko background mein start karega aur foran prompt wapas de dega.

---

# 🔬 Lab 3 – `ls` Ko Background Mein Run Karein

Command:

```bash
ls &
```

Example Output:

```text
[1] 2450
```

Us ke baad command ka output bhi appear ho sakta hai.

---

# Output Ko Samjhein

```text
[1] 2450
```

| Value | Matlab |
|-------|--------|
| `[1]` | Shell Job Number |
| `2450` | Process ID (PID) |

Shell job number ko current terminal ke jobs manage karne ke liye use karta hai.

Operating system process ko identify karne ke liye PID use karta hai.

---

# Background Process Flow

```text
User Command & Enter Karta Hai
        │
        ▼
Shell Background Job Start Karta Hai
        │
        ├── Process Background Mein Continue Karta Hai
        │
        └── Shell Prompt Foran Wapas Aata Hai
                     │
                     ▼
            User Doosri Commands Chala Sakta Hai
```

---

# 7. Foreground Aur Background Processes Ka Farq

| Feature | Foreground Process | Background Process |
|---------|--------------------|--------------------|
| Terminal occupy karta hai | Haan | Nahi |
| Keyboard input receive karta hai | Haan | Aam tor par nahi |
| Output screen par aata hai | Haan | Redirect na ho to aa sakta hai |
| Shell prompt available hota hai | Nahi, jab tak process complete na ho | Haan |
| Default tor par start hota hai | Haan | Nahi |
| Start syntax | `command` | `command &` |
| Best use | Short ya interactive commands | Long-running, non-interactive tasks |

---

# 8. Long-Running Command Ki Practice

Job control practice ke liye `sleep` command bohot useful hai.

Run karein:

```bash
sleep 60
```

Ye process 60 seconds tak foreground mein run karega.

Is dauran:

- Terminal occupied rahega.
- Shell prompt available nahi hoga.

Isay interrupt karne ke liye press karein:

```text
Ctrl + C
```

---

# 🔬 Lab 4 – `sleep` Ko Background Mein Run Karein

Command:

```bash
sleep 60 &
```

Example Output:

```text
[1] 2520
```

Prompt foran wapas aa jayega.

Ab chalayein:

```bash
date
```

`sleep` process background mein chalta rahega jab ke aap doosri command run karenge.

---

# 9. `jobs` Se Background Jobs List Karein

Current shell se start hone wale jobs dekhne ke liye:

```bash
jobs
```

Example Output:

```text
[1]+  Running    sleep 60 &
```

---

# `jobs` Output Ko Samjhein

| Field | Matlab |
|-------|--------|
| `[1]` | Job Number |
| `+` | Current ya default job |
| `Running` | Job state |
| `sleep 60 &` | Command |

---

# 🔬 Lab 5 – Current Jobs Dekhein

Do background jobs start karein:

```bash
sleep 100 &
sleep 200 &
```

Ab run karein:

```bash
jobs
```

Example Output:

```text
[1]-  Running    sleep 100 &
[2]+  Running    sleep 200 &
```

---

# 10. Job Number Aur PID Mein Farq

Linux do identifiers use karta hai:

| Identifier | Example | Kis Ke Liye |
|------------|---------|--------------|
| Job Number | `%1` | Current shell |
| PID | `2520` | Linux Kernel |

Job number sirf us shell session mein hota hai jahan job start hui ho.

PID operating system level par process ko identify karta hai.

---

# 11. Background Job Ko Foreground Mein Lana

`fg` command use karein.

Syntax:

```bash
fg %job_number
```

Example:

```bash
fg %1
```

Ye Job 1 ko foreground mein le aayega.

Terminal ab us job ke zariye occupy ho jayega.

---

# 🔬 Lab 6 – Job Ko Foreground Mein Move Karein

Start karein:

```bash
sleep 300 &
```

Check karein:

```bash
jobs
```

Foreground mein le aayein:

```bash
fg %1
```

Ab terminal `sleep` process ke zariye occupied hai.

Terminate karne ke liye press karein:

```text
Ctrl + C
```

---

# 12. Foreground Process Ko Suspend Karna

Running Foreground Process ko suspend karne ke liye:

```text
Ctrl + Z
```

Ye aam tor par `SIGTSTP` signal bhejta hai.

Example:

```bash
sleep 300
```

Press karein:

```text
Ctrl + Z
```

Expected Output:

```text
[1]+  Stopped    sleep 300
```

Process pause hua hai, terminate nahi hua.

---

# 13. Suspended Job Ko Background Mein Resume Karna

Command:

```bash
bg %1
```

Example:

```bash
bg %1
```

Expected Output:

```text
[1]+ sleep 300 &
```

Suspended job background mein dobara run hona shuru ho jayegi.

---

# Job-Control Flow

```text
Foreground Job
      │
      │ Ctrl + Z
      ▼
Stopped Job
      │
      ├── bg %1 ──► Background
      │
      └── fg %1 ──► Foreground
```

---

# 14. Suspended Job Ko Foreground Mein Resume Karna

Command:

```bash
fg %1
```

Ye job ko foreground mein resume karega.

---

# 🔬 Lab 7 – Job Suspend Aur Resume Karein

Run karein:

```bash
sleep 500
```

Press karein:

```text
Ctrl + Z
```

Check karein:

```bash
jobs
```

Background mein resume karein:

```bash
bg %1
```

Dobarah foreground mein laayein:

```bash
fg %1
```

Terminate karein:

```text
Ctrl + C
```

---

# 15. Important Keyboard Shortcuts

| Shortcut | Kaam |
|----------|------|
| `Ctrl + C` | Foreground Process ko terminate kare |
| `Ctrl + Z` | Foreground Process ko suspend kare |
| `Ctrl + D` | Interactive command ko End-of-File bheje |
| `Ctrl + L` | Terminal screen clear kare |

---

# `Ctrl + C` Aur `Ctrl + Z` Mein Farq

| Shortcut | Result |
|----------|--------|
| `Ctrl + C` | Process ko stop aur terminate karta hai |
| `Ctrl + Z` | Process ko pause karta hai aur stopped job bana deta hai |

---

# 16. Background Output

Background Process phir bhi terminal par output likh sakta hai.

Example:

```bash
for i in {1..5}; do echo "Message $i"; sleep 2; done &
```

Prompt wapas aa jayega, lekin messages screen par aate rahenge.

Is se terminal use karna mushkil ho sakta hai.

---

# 17. Background Output Redirect Karna

Background output ko terminal par aane se rokne ke liye usay file mein redirect karein.

Example:

```bash
./backup.sh > backup.log 2>&1 &
```

### Explanation

| Part | Matlab |
|------|--------|
| `> backup.log` | Standard output ko `backup.log` mein bhejo |
| `2>&1` | Standard error ko bhi usi file mein bhejo |
| `&` | Command background mein run karo |

---

# Ek Aur Example

```bash
find / -name "*.conf" > find-results.txt 2> find-errors.txt &
```

Ye bhejta hai:

- Normal output ko `find-results.txt`
- Errors ko `find-errors.txt`
- Process ko background mein

---

# 18. Background Process Running Hai Ya Nahi, Kaise Check Karein?

Use karein:

```bash
jobs
```

Ya:

```bash
ps
```

Ya:

```bash
ps aux | grep '[s]leep'
```

Ya:

```bash
pgrep -a sleep
```

---

# 19. Background Job Terminate Karna

Job number ke saath `kill` use karein:

```bash
kill %1
```

Ya PID ke saath:

```bash
kill 2520
```

Verify karein:

```bash
jobs
```

---

# 🔬 Lab 8 – Background Job Stop Karein

Start karein:

```bash
sleep 600 &
```

Check karein:

```bash
jobs
```

Job terminate karein:

```bash
kill %1
```

Verify karein:

```bash
jobs
```

---

# 20. Background Job Complete Hone Par Kya Hota Hai?

Jab Background Job complete hoti hai to Bash message dikhata hai:

```text
[1]+  Done    sleep 60
```

Is ka matlab:

- Process complete ho gaya.
- Shell ne job status update kar diya.
- Job ab running nahi hai.

---

# 21. Background Job Aur Logout

Normal background job terminal ya shell close hone par terminate ho sakti hai.

Jo jobs logout ke baad bhi continue karni hon, un ke liye commonly ye tools use hote hain:

```bash
nohup
```

```bash
screen
```

```bash
tmux
```

Production services ke liye shell background jobs ke bajaye:

```text
systemd
```

use karna chahiye.

---

# 22. `nohup` Ke Saath Job Run Karna

Example:

```bash
nohup ./backup.sh > backup.log 2>&1 &
```

Ye process ko terminal disconnect hone ke baad bhi continue karne mein madad karta hai.

Logout ke baad process ko verify karna chahiye kyun ke behavior shell aur environment par depend kar sakta hai.

---

# 23. Practical Example: Backup Script

## Foreground Execution

```bash
./backup.sh
```

Result:

- Terminal occupied rahega.
- Prompt available nahi hoga.
- Completion ka wait karna padega.

---

## Background Execution

```bash
./backup.sh > backup.log 2>&1 &
```

Result:

- Backup background mein run hoga.
- Terminal available rahega.
- Output `backup.log` mein save hogi.
- Doosri commands execute ki ja sakengi.

---

# 24. Complete Job-Control Workflow

```text
Long Command Start Karein
      │
      ├── command ─────────► Foreground
      │                        │
      │                        ├── Ctrl + C ─► Terminated
      │                        │
      │                        └── Ctrl + Z ─► Stopped
      │                                           │
      │                                           ├── bg %1 ─► Background
      │                                           └── fg %1 ─► Foreground
      │
      └── command & ───────► Background
                               │
                               ├── fg %1 ─► Foreground
                               └── kill %1 ─► Terminated
```

---

# 🧪 Practice Exercises

## Exercise 1 – Foreground Job

Run karein:

```bash
sleep 30
```

Observe karein ke terminal occupied hai.

---

## Exercise 2 – Background Job

Run karein:

```bash
sleep 30 &
```

Verify karein ke prompt foran wapas aa gaya.

---

## Exercise 3 – Jobs List Karein

Run karein:

```bash
jobs
```

---

## Exercise 4 – Multiple Jobs Start Karein

```bash
sleep 100 &
sleep 200 &
sleep 300 &
```

List karein:

```bash
jobs
```

---

## Exercise 5 – Job Ko Foreground Mein Laayein

```bash
fg %1
```

---

## Exercise 6 – Foreground Job Suspend Karein

Press karein:

```text
Ctrl + Z
```

---

## Exercise 7 – Background Mein Resume Karein

```bash
bg %1
```

---

## Exercise 8 – Background Job Terminate Karein

```bash
kill %1
```

---

## Exercise 9 – Background Output Redirect Karein

```bash
for i in {1..10}; do echo "Line $i"; sleep 1; done > output.log 2>&1 &
```

Output dekhein:

```bash
tail -f output.log
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – Terminal Occupied Hai

Long-running process foreground mein hai.

Press karein:

```text
Ctrl + Z
```

Phir run karein:

```bash
bg
```

---

### Scenario 2 – Background Job Ko Foreground Mein Lana Hai

Jobs check karein:

```bash
jobs
```

Phir:

```bash
fg %1
```

---

### Scenario 3 – Background Output Terminal Ko Disturb Kar Rahi Hai

Command ko redirection ke saath dobara start karein:

```bash
command > command.log 2>&1 &
```

---

### Scenario 4 – Background Job Stop Karni Hai

Run karein:

```bash
kill %1
```

Ya:

```bash
kill PID
```

---

### Scenario 5 – Logout Ke Baad Job Disappear Ho Gayi

Long-running job ke liye use karein:

```bash
nohup command > output.log 2>&1 &
```

Ya use karein:

```text
screen
tmux
systemd
```

---

# 📌 Quick Revision

| Command Ya Key | Kaam |
|----------------|------|
| `command` | Foreground mein run kare |
| `command &` | Background mein run kare |
| `jobs` | Current shell jobs list kare |
| `fg %1` | Job 1 ko foreground mein laaye |
| `bg %1` | Job 1 ko background mein resume kare |
| `kill %1` | Job 1 ko terminate kare |
| `Ctrl + C` | Foreground process terminate kare |
| `Ctrl + Z` | Foreground process suspend kare |
| `nohup command &` | Logout ke baad bhi command ko continue karne mein madad kare |
| `> file 2>&1 &` | Output redirect karke background mein run kare |

---

# 📖 Key Takeaways

- Commands default tor par foreground mein run hoti hain.
- Foreground Process terminal ko occupy karta hai.
- Background Process terminal ko available rakhta hai.
- Command ke end par `&` lagane se woh background mein start hoti hai.
- `jobs` current shell ke jobs dikhata hai.
- `fg` job ko foreground mein lata hai.
- `bg` suspended job ko background mein resume karta hai.
- `Ctrl + Z` process ko suspend karta hai.
- `Ctrl + C` Foreground Process ko terminate karta hai.
- Background output ko aam tor par file mein redirect karna chahiye.
- Shell jobs temporary tasks ke liye useful hain, jab ke long-running production services ko aam tor par `systemd` ke zariye manage karna chahiye.

---

# 💡 Yaad Rakhein

> **Terminal ko ek Service Counter ki tarah samjhein.**
>
> - **Foreground Process** counter par khara rehta hai aur kaam complete hone tak usay occupy karta hai.
> - **Background Process** apna kaam staff ko de deta hai aur aap ko counter par doosra kaam karne deta hai.
>
> **Golden Rules:**
>
> ```text
> Foreground Job = command
>
> Background Job = command &
>
> Jobs List      = jobs
>
> Foreground Mein Laana = fg %job
>
> Background Mein Bhejna = bg %job
> ```
>
> **Short ya interactive commands ke liye foreground use karein, aur long non-interactive jobs ke liye background execution use karein.**