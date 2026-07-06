# Linux Practice Lab – systemctl ke Sath Services Manage Karna (RHCSA Style)
# Day-60
> Date 07, 2026

## Maqsad (Objective)

Is lab ke baad aap seekh jayenge ke:

- systemd ka purpose samajhna
- Service ka status dekhna
- systemctl status output ko samajhna
- Unit configuration files locate karna
- Services ko start aur stop karna
- Services ko enable aur disable karna
- Enable karne se symbolic link kaise banta hai
- Restart aur Reload ka farq samajhna
- Service ko Mask aur Unmask karna
- Linux boot ke waqt services kaise start hoti hain

---

# Lab Environment

- Operating System: Rocky Linux 9
- User: root (ya sudo user)

---

# Scenario

Aap ne abhi aik company join ki hai jahan aap Linux System Administrator hain.

Aap ki zimmedari hai ke Linux server ki services ko **systemd** ke zariye manage karein.

Is lab mein aap services ka status check karenge, unhein start aur stop karenge, restart aur reload karenge, enable aur disable karenge aur mask aur unmask karna seekhenge bilkul waise hi jaise production environment mein hota hai.

---

# Task 1 – systemctl Manual Dekhna

Manual kholen.

```bash
man systemctl
```

Manual ke andar kisi word ko search karein.

```
/restart
```

Agla result dekhne ke liye.

```
n
```

Pichla result dekhne ke liye.

```
N
```

Manual se bahar nikalne ke liye.

```
q
```

---

# Task 2 – sshd Service ka Status Dekhna

Command chalayein.

```bash
systemctl status sshd.service
```

Example

```
Loaded

Active

Main PID

Tasks

Memory

CPU
```

---

# Output ko Samajhna

## Loaded

Example

```
Loaded:

loaded (/usr/lib/systemd/system/sshd.service; enabled)
```

### Matlab

Linux ne is service ki configuration file load kar li hai.

Notice karein ke configuration file kis location par hai.

```
/usr/lib/systemd/system/
```

Yahin par zyadatar system supplied unit files mojood hoti hain.

---

## Active

```
Active: active (running)
```

### Matlab

Service is waqt chal rahi hai.

---

## Enabled

```
enabled
```

### Matlab

Jab Linux boot hoga to ye service automatically start hogi.

---

## Main PID

```
Main PID: 770
```

### Matlab

Ye is running service ka Process ID hai.

---

## Tasks

Ye batata hai ke is service ke kitne processes ya threads chal rahe hain.

---

## Memory

Ye batata hai ke service kitni RAM use kar rahi hai.

---

## CPU

Ye batata hai ke service ne kitna CPU time consume kiya hai.

---

# Task 3 – Dusri Service ka Status Dekhna

Ab rsyslog service check karein.

```bash
systemctl status rsyslog
```

Observe karein.

- Loaded
- Active
- PID
- Memory
- CPU
- Logs

Neeche recent logs bhi nazar aayenge jo is service se related hain.

---

# Task 4 – Unit Configuration Files Dekhna

Directory ke andar jayen.

```bash
cd /usr/lib/systemd/system
```

Files dekhein.

```bash
ls -ltr
```

Aap ko mukhtalif unit types nazar aayenge.

Example

```
logrotate.service

logrotate.timer

dnf-makecache.service

dnf-makecache.timer

sshd.service

multi-user.target

graphical.target
```

---

# Unit Types ko Samajhna

| Unit | Matlab |
|------|---------|
| .service | Service |
| .timer | Scheduler |
| .target | Boot Target |
| .socket | Socket Activation |
| .mount | Mount Point |
| .device | Device |
| .path | Kisi file ya directory ko monitor karna |

---

# Task 5 – Service Stop Karna

rsyslog service ko stop karein.

```bash
systemctl stop rsyslog
```

Verify karein.

```bash
systemctl status rsyslog
```

Expected

```
Active: inactive (dead)
```

### Sawal

Kya configuration file delete ho gayi?

Jawab

Nahi.

Sirf running process band hua hai.

---

# Task 6 – Service Dobarah Start Karna

```bash
systemctl start rsyslog
```

Verify karein.

```bash
systemctl status rsyslog
```

Expected

```
Active: active (running)
```

---

# Task 7 – Service Disable Karna

Service disable karein.

```bash
systemctl disable rsyslog
```

Output

```
Removed

/etc/systemd/system/multi-user.target.wants/rsyslog.service
```

---

# Kya Hua?

Linux ne aik symbolic link remove kar diya.

Service delete nahi hui.

Lekin ab jab system boot hoga to ye automatically start nahi hogi.

Agar aap service use karna chahein to manually start karni hogi.

---

# Verify Karein

```bash
systemctl status rsyslog
```

Notice karein.

```
disabled
```

---

# Task 8 – Service Dobarah Enable Karna

```bash
systemctl enable rsyslog
```

Output

```
Created symlink

/etc/systemd/system/multi-user.target.wants/rsyslog.service
```

Status check karein.

```bash
systemctl status rsyslog
```

Notice karein.

```
enabled
```

---

# Task 9 – Symbolic Link Dekhna

Command chalayein.

```bash
ls -l /etc/systemd/system/multi-user.target.wants/
```

Yahan rsyslog.service ka symbolic link dekhein.

### Sawal

Ye symbolic link kis file ki taraf point kar raha hai?

---

# Task 10 – Linux init Process Dekhna

Command chalayein.

```bash
ls -l /sbin/init
```

Output

```
/sbin/init -> ../lib/systemd/systemd
```

### Sawal

Is se kya pata chalta hai?

Jawab

Rocky Linux 9 mein **systemd hi PID 1 hai** aur kernel ke baad sab se pehla userspace process hota hai.

---

# Task 11 – Service Restart Karna

Sab se pehle PID note karein.

```bash
systemctl status rsyslog
```

Example

```
Main PID: 81322
```

Restart karein.

```bash
systemctl restart rsyslog
```

Dobarah status check karein.

```bash
systemctl status rsyslog
```

Example

```
Main PID: 81331
```

### Sawal

Kya PID change hui?

Jawab

Ji Haan.

Restart service ko pehle stop karta hai phir dobarah start karta hai.

Isi liye naya PID milta hai.

---

# Task 12 – Service Reload Karna

Reload karein.

```bash
systemctl reload rsyslog
```

Status check karein.

```bash
systemctl status rsyslog
```

Notice karein.

```
ExecReload

Main PID: 81331
```

### Sawal

Kya PID change hui?

Jawab

Nahi.

Reload sirf configuration files dobarah load karta hai.

Service band nahi hoti.

---

# Restart aur Reload ka Farq

| Restart | Reload |
|----------|---------|
| Service stop hoti hai | Service stop nahi hoti |
| Dobarah start hoti hai | Sirf configuration reload hoti hai |
| PID change hota hai | PID same rehta hai |
| Chhoti interruption hoti hai | Aksar koi interruption nahi hoti |

---

# Task 13 – reload-or-restart

Command chalayein.

```bash
systemctl reload-or-restart rsyslog
```

### Matlab

Agar service reload support karti hai to reload karegi.

Agar reload support nahi karti to restart karegi.

Ye production environments mein bohot useful command hai.

---

# Task 14 – Service Mask Karna

Sab se pehle service stop karein.

```bash
systemctl stop rsyslog
```

Ab service ko mask karein.

```bash
systemctl mask rsyslog
```

Output

```
Created symlink

/etc/systemd/system/rsyslog.service

->

/dev/null
```

---

# Mask ka Matlab

Masking ka matlab hai ke service ko itna block kar diya gaya hai ke koi administrator bhi usay accidentally start nahi kar sakta.

---

# Verify Karein

```bash
systemctl status rsyslog
```

Notice karein.

```
Loaded:

masked
```

---

# Task 15 – Masked Service Ko Start Karne Ki Koshish Karein

```bash
systemctl start rsyslog
```

Expected

```
Failed

Unit is masked.
```

---

# Task 16 – Service Unmask Karna

Command chalayein.

```bash
systemctl unmask rsyslog
```

Output

```
Removed

/etc/systemd/system/rsyslog.service
```

Ab service dobarah start karein.

```bash
systemctl start rsyslog
```

Verify karein.

```bash
systemctl status rsyslog
```

Expected

```
Active:

active (running)
```

---

# Summary Table

| Command | Matlab |
|----------|---------|
| systemctl status | Service ka status dekhna |
| systemctl start | Service start karna |
| systemctl stop | Service stop karna |
| systemctl restart | Service dobarah start karna |
| systemctl reload | Configuration reload karna |
| systemctl reload-or-restart | Reload agar mumkin ho warna restart |
| systemctl enable | Boot ke waqt automatically start karna |
| systemctl disable | Boot ke waqt automatically start na karna |
| systemctl mask | Service ko poori tarah block karna |
| systemctl unmask | Block hata dena |

---

# RHCSA Challenge 1

Neeche di hui services ka status check karein.

```
sshd

firewalld

chronyd
```

Likhein.

- Active
- PID
- Memory
- CPU

---

# RHCSA Challenge 2

Directory ke andar se teen timer units dhoondhein.

```
/usr/lib/systemd/system
```

---

# RHCSA Challenge 3

rsyslog ko disable karein.

Status verify karein.

Phir dobarah enable karein.

---

# RHCSA Challenge 4

rsyslog restart karein.

PID note karein.

Dobarah restart karein.

Kya PID badal gayi?

---

# RHCSA Challenge 5

rsyslog reload karein.

Kya PID badli?

Wajah likhein.

---

# RHCSA Challenge 6

rsyslog ko mask karein.

Usay start karne ki koshish karein.

Error note karein.

---

# RHCSA Challenge 7

Service ko unmask karein.

Dobarah successfully start karein.

---

# Knowledge Check

1. System unit files kis directory mein hoti hain?

2. Enabled ka kya matlab hai?

3. Disabled ka kya matlab hai?

4. Masked ka kya matlab hai?

5. Service start karne ki command kya hai?

6. Service stop karne ki command kya hai?

7. Service restart karne ki command kya hai?

8. Configuration reload karne ki command kya hai?

9. Service ko boot ke waqt automatically start karne ki command kya hai?

10. Service ko permanently block karne ki command kya hai?

11. PID 1 kaunsa process hota hai?

12. Rocky Linux 9 mein `/sbin/init` kis file ki taraf point karta hai?

---

# Summary

Is lab ke baad aap confidently:

✅ systemctl status ko samajh sakte hain.

✅ Unit configuration files locate kar sakte hain.

✅ Services start aur stop kar sakte hain.

✅ Services enable aur disable kar sakte hain.

✅ Enable ke symbolic links ko samajh sakte hain.

✅ Restart aur Reload ka farq explain kar sakte hain.

✅ Mask aur Unmask use kar sakte hain.

✅ Samajh sakte hain ke **systemd** Linux ki services ko kis tarah manage karta hai.

Ye tamam skills RHCSA exam ke liye bohot important hain aur real production Linux servers mein rozana istemal hoti hain.