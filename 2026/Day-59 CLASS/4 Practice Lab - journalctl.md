# MODULE 06/07 – Practice Lab: Working with `journalctl`
> **Troubleshooting Ke Liye System Information Collect Karna (Roman Urdu)**

---

# Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- `journalctl` ka purpose samajhna.
- System Journal logs ko dekhna.
- Current aur Previous Boot ke logs dekhna.
- System ke tamam recorded boots ki list dekhna.
- Real-time mein logs monitor karna.
- Kisi specific systemd service (Unit) ke logs dekhna.
- Priority (Severity) ke mutabiq logs filter karna.
- Volatile aur Persistent Journal logs ka farq samajhna.

---

# Introduction

Jab bhi koi user system par koi problem report karta hai, to Linux System Administrator ki pehli zimmedari hoti hai ke **jitni mumkin ho sake utni information collect kare**, us ke baad hi troubleshooting shuru kare.

Misal ke taur par:

- Error kya tha?
- Error kab aya?
- Konsi service fail hui?
- Kya system recently reboot hua tha?
- Kya authentication failure hui thi?
- Kya kernel error aya tha?

Guess karne ke bajaye administrator logs ko analyze karta hai taake problem ki asal wajah maloom ho sake.

---

# Linux Logging Ko Samjhein

Linux mein do main logging services hoti hain.

| Service | Kaam |
|----------|------|
| `systemd-journald` | Journal logs collect aur store karta hai |
| `rsyslog` | Logs ko permanently `/var/log` mein store karta hai |

---

# systemd-journald aur rsyslog Mein Farq

| Feature | systemd-journald | rsyslog |
|----------|------------------|----------|
| Storage Location | Memory (RAM) by default | Disk |
| Reboot Ke Baad Logs | Nahin (jab tak Persistent configure na ho) | Haan |
| Tool | `journalctl` | `less`, `tail`, `cat` |
| Log Format | Binary | Plain Text |

---

# `journalctl` Kya Hai?

`journalctl` ek standard Linux utility hai jo **systemd-journald** ke collect kiye hue logs ko read karne ke liye use hoti hai.

Kyunkay journal logs binary format mein hote hain, is liye unhein `cat` ya `less` se directly read nahi kiya ja sakta.

Is ke liye Linux `journalctl` command provide karta hai.

Basic syntax:

```bash
journalctl
```

Ye command tamam available journal messages display karti hai.

---

# Step 1 – Tamam Journal Logs Display Karein

```bash
journalctl
```

Ye command system ke tamam journal messages display karegi.

---

# Step 2 – Current Boot Ke Logs Dekhein

Current boot ke tamam messages dekhne ke liye:

```bash
journalctl -b
```

Yahan:

- `-b` = Boot

Ye current boot ke baad generate hone wale tamam logs display karega.

---

# Step 3 – Previous Boot Ke Logs Dekhein

Previous boot ke logs dekhne ke liye:

```bash
journalctl -b -1
```

Do boots pehle ke logs:

```bash
journalctl -b -2
```

Aap jitne purane boot dekhna chahein utna number de sakte hain.

---

# Step 4 – Available Boots Ki List Dekhein

System ke tamam recorded boots dekhne ke liye:

```bash
journalctl --list-boots
```

Example Output:

```text
-2
-1
 0
```

Yahan:

- `0` = Current Boot
- `-1` = Previous Boot
- `-2` = Do Boots Pehle

---

# Step 5 – Real-Time Logs Monitor Karein

Naye log messages continuously dekhne ke liye:

```bash
journalctl -f
```

Ya

```bash
journalctl -ef
```

Explanation:

- `-f` = Follow Mode
- `-e` = End Se Start Karo

Ye command bilkul:

```bash
tail -f
```

ki tarah kaam karti hai.

Jaise hi koi naya log generate hoga, foran screen par nazar aa jayega.

---

# Practice

Ek naya terminal open karein.

Kisi doosre user se login karein ya koi service restart karein.

Dekhein ke naye log entries foran screen par appear hoti hain.

---

# Step 6 – Kisi Specific Service Ke Logs Dekhein

Agar kisi particular systemd service ke logs dekhne hon to:

```bash
journalctl -u sshd.service
```

Ya

```bash
journalctl --unit=sshd.service
```

Ye sirf SSH service ke logs dikhayega.

Doosri examples:

```bash
journalctl -u NetworkManager.service
```

```bash
journalctl -u firewalld.service
```

```bash
journalctl -u crond.service
```

---

# Step 7 – Priority Ke Hisaab Se Logs Dekhein

Journal logs ko severity ke mutabiq bhi filter kiya ja sakta hai.

Example:

```bash
journalctl -p err
```

Ye sirf Error messages display karega.

---

## Priority Levels

| Priority | Matlab |
|-----------|---------|
| emerg | Emergency |
| alert | Alert |
| crit | Critical |
| err | Error |
| warning | Warning |
| notice | Notice |
| info | Information |
| debug | Debug |

---

## Priority Range Display Karna

Example:

```bash
journalctl -p err..alert
```

Ye display karega:

- Error
- Critical
- Alert

sirf yehi messages.

---

# Step 8 – Persistent Journal Logs

Default tor par `systemd-journald` logs ko Memory (RAM) mein store karta hai.

Is ka matlab:

- System reboot hua.
- Purane logs khatam ho gaye.

Inhein kehte hain:

> **Volatile Journal Logs**

---

## Persistent Journal

Agar Journal ko Persistent configure kar diya jaye:

To logs Disk par save honge.

Is ke faide:

- Reboot ke baad bhi logs mojood rahenge.
- Previous Boot ke logs available honge.
- Troubleshooting aasaan ho jayegi.

Persistent Journal ka default location hota hai:

```text
/var/log/journal
```

---

# 🔬 Practice Exercises

---

## Exercise 1

Tamam Journal Logs display karein.

```bash
journalctl
```

---

## Exercise 2

Current Boot ke logs dekhein.

```bash
journalctl -b
```

---

## Exercise 3

Previous Boot ke logs dekhein.

```bash
journalctl -b -1
```

---

## Exercise 4

Tamam Recorded Boots ki list dekhein.

```bash
journalctl --list-boots
```

---

## Exercise 5

Real-Time Logs monitor karein.

```bash
journalctl -ef
```

Ek naya terminal open karein.

Doosre user se login karein.

Observe karein ke naye logs foran appear ho rahe hain.

---

## Exercise 6

SSH Service ke logs dekhein.

```bash
journalctl -u sshd.service
```

---

## Exercise 7

Cron Service ke logs dekhein.

```bash
journalctl -u crond.service
```

---

## Exercise 8

Sirf Error messages dekhein.

```bash
journalctl -p err
```

---

## Exercise 9

Error se Alert tak ke messages dekhein.

```bash
journalctl -p err..alert
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

User report karta hai:

> "Main SSH se server par login nahi kar pa raha."

Command:

```bash
journalctl -u sshd.service
```

---

### Scenario 2

Koi Service Start nahi ho rahi.

Command:

```bash
journalctl -u httpd.service
```

---

### Scenario 3

Server achanak reboot ho gaya.

Command:

```bash
journalctl -b -1
```

---

### Scenario 4

Users report kar rahe hain ke is waqt errors aa rahe hain.

Command:

```bash
journalctl -ef
```

---

### Scenario 5

Sirf Error messages dekhne hain.

Command:

```bash
journalctl -p err
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `journalctl` | Tamam Journal Logs display kare |
| `journalctl -b` | Current Boot ke logs |
| `journalctl -b -1` | Previous Boot ke logs |
| `journalctl --list-boots` | Tamam Boots ki list |
| `journalctl -f` | Live Logs Follow Kare |
| `journalctl -ef` | End se Start karke Live Follow Kare |
| `journalctl -u SERVICE` | Kisi specific service ke logs |
| `journalctl -p err` | Sirf Error messages |
| `journalctl -p err..alert` | Error se Alert tak ke messages |

---

# 📖 Key Takeaways

- `journalctl` ko **systemd-journald** ke collect kiye hue logs read karne ke liye use kiya jata hai.
- Default tor par Journal Logs Memory mein store hote hain aur reboot ke baad khatam ho sakte hain.
- Persistent Journal configure karne se logs reboot ke baad bhi available rehte hain.
- `journalctl` ki madad se logs ko filter kiya ja sakta hai:

  - Boot
  - Service
  - Priority
  - Time

- `journalctl -ef` real-time troubleshooting ke liye bohot powerful command hai.

---

# 💡 Yaad Rakhein

> **`journalctl` ko Linux System Logs ka Google Search Engine samjhein.**
>
> Jaise Google internet par information dhoondhta hai, waise hi `journalctl` Linux ke logs ko search aur filter karta hai.
>
> Aap bohot aasani se logs ko filter kar sakte hain:
>
> - Boot ke hisaab se
> - Service ke hisaab se
> - Severity ke hisaab se
> - Time ke hisaab se
>
> Is wajah se troubleshooting bohot tez aur aasaan ho jati hai.