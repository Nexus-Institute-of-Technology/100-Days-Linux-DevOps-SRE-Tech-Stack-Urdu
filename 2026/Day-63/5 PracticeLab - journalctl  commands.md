# MODULE 06/07 – Practice Lab: Advanced `journalctl` Commands
> **Hands-on Practice Lab – Boot Logs, Time-Based Logs, Verbose Output aur Persistent Journal (Roman Urdu)**

---

# Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- Boot se related journal logs dekhna.
- Previous boot ke logs dekhna.
- System ke tamam recorded boots ki list dekhna.
- Kisi specific date aur time ke darmiyan ke logs dekhna.
- Verbose output display karna.
- Journal logs kis location par store hote hain.
- Persistent Journal Storage configure karna.
- Reboot ke baad persistent journal logs verify karna.

---

# Introduction

Linux System Administrator ki sab se important troubleshooting skills mein se ek yeh hai ke **sahi logs ko jaldi dhoondhna aur analyze karna.**

`journalctl` command bohot powerful hai aur is ki madad se hum logs ko filter kar sakte hain:

- Boot ke hisaab se
- Date ke hisaab se
- Time ke hisaab se
- Service ke hisaab se
- Priority ke hisaab se
- Output format ke hisaab se

Is wajah se troubleshooting bohot aasaan ho jati hai.

---

# 🔬 Lab 1 – Current Boot Ke Logs Dekhna

Current boot ke tamam logs dekhne ke liye command chalayein:

```bash
journalctl -b
```

### Explanation

`-b`

ka matlab hai:

```text
Boot
```

Ye command system ke last boot ke baad generate hone wale tamam journal messages display karti hai.

Ye command khas tor par useful hai jab:

- System crash hua ho.
- Unexpected reboot hua ho.
- Boot failure hui ho.
- Kernel panic aya ho.

---

# 🔬 Lab 2 – Available Boots Ki List Dekhna

Tamam recorded boots ki list dekhne ke liye:

```bash
journalctl --list-boots
```

Example Output:

```text
-1  8f76a2...
 0  94ab31...
```

### Explanation

| Boot ID | Matlab |
|----------|---------|
| `0` | Current Boot |
| `-1` | Previous Boot |
| `-2` | Do Boots Pehle |

Agar sirf:

```text
0
```

show ho raha ho to iska matlab hai:

- Journal Persistent nahi hai.
- Previous Boot ke logs delete ho chuke hain.

---

# 🔬 Lab 3 – Kisi Specific Boot Ke Logs Dekhna

Current Boot:

```bash
journalctl -b 0
```

Previous Boot:

```bash
journalctl -b -1
```

Do Boots Pehle:

```bash
journalctl -b -2
```

Ye command bohot useful hai jab problem reboot se pehle hui ho.

---

# 🔬 Lab 4 – Kisi Specific Date Aur Time Ke Logs Dekhna

Kabhi user report karta hai:

- "Server kal slow tha."
- "Issue subah 8:30 baje shuru hua."
- "Application 2 PM aur 3 PM ke darmiyan fail hui."

Aisi situation mein tamam logs dekhne ki zarurat nahi.

Sirf required time ke logs dekhein.

Example:

```bash
journalctl --since "2026-01-29"
```

Specific Date aur Time:

```bash
journalctl --since "2026-01-29 08:30:00"
```

Start aur End Time dono specify karna:

```bash
journalctl \
--since "2026-01-29 08:30:00" \
--until "2026-01-30 08:30:00"
```

Ye sirf us period ke logs display karega.

---

# 🔬 Lab 5 – Verbose Output Dekhna

Journal ke tamam available fields dekhne ke liye:

```bash
journalctl -o verbose
```

### Explanation

`-o`

ka matlab hai:

```text
Output Format
```

Aur

```text
verbose
```

ka matlab hai:

Har journal entry ki tamam details display karna.

Ye command advanced troubleshooting ke liye bohot useful hai.

---

# 🔬 Lab 6 – Journal Logs Kahan Store Hote Hain?

Default tor par journal logs yahan store hote hain:

```text
/run/log/journal
```

Ya

```text
/run/systemd/journal
```

Ye dono locations **tmpfs (Temporary File System)** par hoti hain.

Check karne ke liye:

```bash
df -h
```

Aap dekhenge ke `/run` ek **tmpfs** filesystem hai.

Is ka matlab:

- Logs RAM mein store hote hain.
- Reboot ke baad tamam logs delete ho jate hain.

Inhein kehte hain:

> **Volatile Journal Logs**

---

# 🔬 Lab 7 – Persistent Journal Storage Configure Karna

Agar aap chahte hain ke reboot ke baad bhi logs mojood rahen to Permanent Journal create karein.

Directory change karein:

```bash
cd /var/log
```

Journal directory banayein:

```bash
mkdir journal
```

---

# Step 2 – Ownership Set Karein

```bash
chown root:systemd-journal /var/log/journal
```

---

# Step 3 – Permissions Set Karein

```bash
chmod 2755 /var/log/journal
```

### Explanation

`2755` ka matlab:

- Owner = Read, Write, Execute
- Group = Read aur Execute
- Others = Read aur Execute
- SetGID Bit Enable

---

# Step 4 – systemd-journald Service Restart Karein

```bash
systemctl restart systemd-journald
```

Status verify karein:

```bash
systemctl status systemd-journald
```

Expected Output:

```text
Active: active (running)
```

---

# Step 5 – System Reboot Karein

```bash
reboot
```

System ke dobara start hone ka wait karein.

---

# Step 6 – Persistent Journal Verify Karein

Reboot ke baad:

```bash
journalctl --list-boots
```

Ab multiple boots show honi chahiye.

Example:

```text
-1
 0
```

Ye confirm karta hai ke Journal ab Persistent ho chuka hai.

---

# Step 7 – Previous Boot Ke Logs Dekhein

Current Boot:

```bash
journalctl -b 0
```

Previous Boot:

```bash
journalctl -b -1
```

Ab reboot se pehle ke logs bhi available honge.

---

# 🧪 Practice Exercises

---

## Exercise 1

Current Boot ke logs dekhein.

```bash
journalctl -b
```

---

## Exercise 2

Tamam recorded boots ki list dekhein.

```bash
journalctl --list-boots
```

---

## Exercise 3

Previous Boot ke logs dekhein.

```bash
journalctl -b -1
```

---

## Exercise 4

Specific date ke baad ke logs dekhein.

```bash
journalctl --since "2026-01-29"
```

---

## Exercise 5

Do dates ke darmiyan ke logs dekhein.

```bash
journalctl \
--since "2026-01-29 08:30:00" \
--until "2026-01-30 08:30:00"
```

---

## Exercise 6

Verbose output dekhein.

```bash
journalctl -o verbose
```

---

## Exercise 7

Persistent Journal directory create karein.

```bash
mkdir /var/log/journal
```

---

## Exercise 8

Ownership set karein.

```bash
chown root:systemd-journal /var/log/journal
```

---

## Exercise 9

Permissions set karein.

```bash
chmod 2755 /var/log/journal
```

---

## Exercise 10

Journal service restart karein.

```bash
systemctl restart systemd-journald
```

---

## Exercise 11

Server reboot karein.

```bash
reboot
```

---

## Exercise 12

Persistent Journal verify karein.

```bash
journalctl --list-boots
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Server kal crash hua tha.

Command:

```bash
journalctl -b -1
```

---

### Scenario 2

User report karta hai ke subah 8:30 aur 10:00 ke darmiyan issue tha.

Command:

```bash
journalctl \
--since "2026-01-29 08:30:00" \
--until "2026-01-29 10:00:00"
```

---

### Scenario 3

Detailed Journal information dekhni hai.

Command:

```bash
journalctl -o verbose
```

---

### Scenario 4

Server reboot hua aur purane logs gayab ho gaye.

**Solution:**

Persistent Journal Storage configure karein.

---

#  Quick Revision

| Command | Kaam |
|----------|------|
| `journalctl -b` | Current Boot ke logs |
| `journalctl -b -1` | Previous Boot ke logs |
| `journalctl --list-boots` | Tamam recorded boots ki list |
| `journalctl --since` | Specific date/time ke baad ke logs |
| `journalctl --until` | Specific date/time tak ke logs |
| `journalctl -o verbose` | Detailed verbose output |
| `mkdir /var/log/journal` | Persistent Journal directory create kare |
| `chown root:systemd-journal /var/log/journal` | Ownership set kare |
| `chmod 2755 /var/log/journal` | Permissions set kare |
| `systemctl restart systemd-journald` | Journal service restart kare |

---

# Key Takeaways

- `journalctl` ki madad se logs ko **Boot**, **Date**, **Time**, aur **Output Format** ke hisaab se search kiya ja sakta hai.
- Default tor par Journal logs RAM mein store hote hain aur reboot ke baad delete ho jate hain.
- `/var/log/journal` directory create karne se Journal Persistent ban jata hai.
- Persistent Journal ki wajah se reboot ke baad bhi purane logs available rehte hain.
- Ye feature historical troubleshooting aur system crash analysis ke liye bohot important hai.

---

#  Yaad Rakhein

> **`journalctl` ko Linux System Logs ka Google Search Engine samjhein.**
>
> Jaise Google internet par information search karta hai, waise hi `journalctl` Linux ke logs ko bohot aasani se search aur filter karta hai.
>
> Aap logs ko filter kar sakte hain:
>
> - Boot ke hisaab se
> - Date ke hisaab se
> - Time ke hisaab se
> - Service ke hisaab se
> - Priority ke hisaab se
> - Output Format ke hisaab se
>
> **Persistent Journal Storage enable karne se reboot ke baad bhi tamam important logs mehfooz rehte hain, jo troubleshooting ko bohot aasaan bana deta hai.**