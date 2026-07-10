# Linux Practice Lab – Service Units ko Manage aur View Karna (RHCSA Style)

## Maqsad (Objective)

Is lab ke baad aap seekh jayenge ke:

- Running service units ki list dekhna.
- `systemctl list-units` ka output samajhna.
- Installed service unit files dekhna.
- Mukhtalif unit file states ko samajhna.
- Check karna ke service active hai ya nahi.
- Check karna ke service enabled hai ya nahi.
- Kisi specific service ka detailed status dekhna.
- Troubleshooting ke liye failed services ki list dekhna.

---

# Lab Environment

- Operating System: Rocky Linux 9
- User: root (ya sudo user)

---

# Scenario

Aap aik Linux System Administrator hain.

Rozana ke kaamon mein se aik important kaam Linux services ko monitor aur troubleshoot karna hota hai.

Is lab mein aap **systemctl** command ki madad se running services, installed unit files, service status, enabled state aur failed services dekhna seekhenge.

---

# Task 1 – Running Service Units ki List Dekhna

Tamam loaded aur running services dekhein.

```bash
systemctl list-units --type=service
```

Example Output

```text
UNIT                             LOAD   ACTIVE   SUB      DESCRIPTION

auditd.service                   loaded active   running  Security Auditing Service

chronyd.service                  loaded active   running  NTP client/server

crond.service                    loaded active   running  Command Scheduler

docker.service                   loaded active   running  Docker Application Container Engine

sshd.service                     loaded active   running  OpenSSH Server
```

---

# Output ko Samajhna

Output mein kai important columns hote hain.

| Column | Matlab |
|---------|---------|
| UNIT | Service ka naam |
| LOAD | Kya unit file memory mein successfully load hui hai |
| ACTIVE | Service ki high-level state |
| SUB | Service ki detailed state |
| DESCRIPTION | Service ka chhota sa description |

---

# Har Column ko Samajhna

## UNIT

Ye service ka naam dikhata hai.

Example

```
sshd.service
```

---

## LOAD

Example

```
loaded
```

### Matlab

systemd ne service ki configuration file successfully load kar li hai.

---

## ACTIVE

Example

```
active
```

### Matlab

Ye service ki overall state batata hai.

Common values:

- active
- inactive
- failed

---

## SUB

Example

```
running
```

Ye service ki detailed state batata hai.

Common values:

- running
- exited
- dead
- failed

---

## DESCRIPTION

Ye service ka short description hota hai.

Example

```
OpenSSH Server
```

---

# Task 2 – Loaded aur Active Units Dekhna

Command chalayein.

```bash
systemctl
```

Ye command sirf wohi units dikhati hai jo:

- Loaded hain
- Active hain

---

# Task 3 – Installed Service Unit Files Dekhna

Tamam installed service unit files dekhein.

```bash
systemctl list-unit-files --type=service
```

Example

```text
UNIT FILE                 STATE       PRESET

auditd.service            enabled     enabled

chronyd.service           enabled     enabled

containerd.service        disabled    disabled

crond.service             enabled     enabled

dbus-broker.service       enabled     enabled
```

---

# Output ko Samajhna

Ye command sirf running services nahi dikhati.

Balke tamam installed service unit files dikhati hai, chahe woh running hon ya na hon.

---

# Columns ko Samajhna

| Column | Matlab |
|---------|---------|
| UNIT FILE | Service unit file ka naam |
| STATE | Current state |
| PRESET | Vendor ka recommended default state |

---

# Common Unit States

## enabled

Service boot ke waqt automatically start hogi.

---

## disabled

Service boot ke waqt automatically start nahi hogi.

Lekin manually start ki ja sakti hai.

---

## static

Is service ko directly enable nahi kiya ja sakta.

Ye kisi doosri service ya target ke zariye start hoti hai.

---

## masked

Service ko poori tarah block kar diya gaya hai.

Jab tak unmask nahi karenge tab tak start nahi ho sakti.

---

# Task 4 – Kisi Specific Service ka Status Dekhna

sshd service ka status dekhein.

```bash
systemctl status sshd.service
```

Observe karein.

- Loaded
- Active
- PID
- Memory
- CPU
- Recent Logs

---

# Task 5 – Check Karein Service Active Hai Ya Nahi

Command chalayein.

```bash
systemctl is-active sshd.service
```

Example Output

```text
active
```

Possible Outputs

```
active

inactive

failed
```

---

# Task 6 – Check Karein Service Enabled Hai Ya Nahi

Command chalayein.

```bash
systemctl is-enabled sshd.service
```

Example Output

```text
enabled
```

Possible Outputs

```
enabled

disabled

masked

static
```

---

# Task 7 – Service Disable Karna (Optional)

Service disable karein.

```bash
systemctl disable sshd
```

Verify karein.

```bash
systemctl is-enabled sshd
```

Expected

```text
disabled
```

Dobarah enable karein.

```bash
systemctl enable sshd
```

Verify karein.

```bash
systemctl is-enabled sshd
```

Expected

```text
enabled
```

> **Note:** Ye task sirf lab environment mein karein. Production server par SSH disable karne se reboot ke baad remote access band ho sakti hai.

---

# Task 8 – Failed Services Dekhna

Failed services ki list dekhein.

```bash
systemctl --failed --type=service
```

Example

```text
UNIT LOAD ACTIVE SUB DESCRIPTION

0 loaded units listed.
```

Agar koi service fail hui hogi to woh is list mein nazar aayegi.

---

# Ye Command Itni Important Kyun Hai?

Troubleshooting ke waqt ye command bohot useful hoti hai.

Jab koi user kahe ke service kaam nahi kar rahi, to Linux administrator sab se pehle ye command chalata hai.

```bash
systemctl --failed --type=service
```

Ye foran batati hai ke kaunsi services failed hain.

---

# Important Commands ka Comparison

| Command | Matlab |
|----------|---------|
| systemctl | Active loaded units dikhata hai |
| systemctl list-units --type=service | Running service units ki list |
| systemctl list-unit-files --type=service | Installed service files ki list |
| systemctl status service | Detailed service status |
| systemctl is-active service | Check kare service running hai ya nahi |
| systemctl is-enabled service | Check kare service boot par start hogi ya nahi |
| systemctl --failed --type=service | Failed services ki list |

---

# RHCSA Challenge 1

Running services ki list dekhein.

```bash
systemctl list-units --type=service
```

Count karein kitni services running hain.

---

# RHCSA Challenge 2

Neeche di hui services ka status check karein.

- sshd
- firewalld
- chronyd
- crond

Likhein.

- Active
- SUB
- Description

---

# RHCSA Challenge 3

Tamam installed service unit files dekhein.

Count karein kitni services hain:

- enabled
- disabled
- static
- masked

---

# RHCSA Challenge 4

Check karein ke ye services enabled hain ya nahi.

- sshd
- chronyd
- firewalld
- crond

---

# RHCSA Challenge 5

Check karein ke ye services active hain ya nahi.

- sshd
- chronyd
- firewalld

---

# RHCSA Challenge 6

Failed services ki list dekhein.

Agar koi failed service nahi hai to explain karein ke output ka kya matlab hai.

---

# Knowledge Check

1. Running service units dekhne ki command kya hai?

2. Installed service unit files dekhne ki command kya hai?

3. `list-units` aur `list-unit-files` mein kya farq hai?

4. LOAD column kya batata hai?

5. ACTIVE column kya batata hai?

6. SUB column kya batata hai?

7. DESCRIPTION column kya batata hai?

8. Service active hai ya nahi check karne ki command kya hai?

9. Service enabled hai ya nahi check karne ki command kya hai?

10. Failed services dekhne ki command kya hai?

11. Enabled ka kya matlab hai?

12. Disabled ka kya matlab hai?

13. Static ka kya matlab hai?

14. Masked ka kya matlab hai?

---

# Summary

Is lab ke baad aap confidently:

✅ Running services ki list dekh sakte hain.

✅ Installed service unit files dekh sakte hain.

✅ LOAD, ACTIVE, SUB aur DESCRIPTION columns ko samajh sakte hain.

✅ Check kar sakte hain ke service active hai ya nahi.

✅ Check kar sakte hain ke service enabled hai ya nahi.

✅ Detailed service status dekh sakte hain.

✅ Failed services identify karke troubleshooting kar sakte hain.

Ye tamam commands RHCSA exam ke liye bohot important hain aur real-world Linux administration mein rozana istemal hoti hain.