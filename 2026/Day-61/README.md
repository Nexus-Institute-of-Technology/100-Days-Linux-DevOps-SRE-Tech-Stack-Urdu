# Linux Practice Lab – systemd Targets ko Samajhna (RHCSA Style)
# Day-61
> DATE JULY 08, 2026
## Maqsad (Objective)

Is lab ke baad aap seekh jayenge ke:

- systemd target kya hota hai.
- Targets aur purane runlevels ke darmiyan kya relation hai.
- System kis default target mein boot hota hai.
- Target unit files kahan hoti hain.
- Mukhtalif systemd targets ko samajhna.
- Kisi target ki dependencies dekhna.
- `systemctl list-dependencies` ka output samajhna.
- Active, inactive aur failed units ka matlab samajhna.

---

# Lab Environment

- Operating System: Rocky Linux 9
- User: root (ya sudo user)

---

# Scenario

Aap aik Linux System Administrator hain aur aap ki zimmedari hai ke Linux boot process ko samjhein.

Modern Linux distributions purane **runlevels** ki jagah **systemd targets** use karti hain.

Is lab mein aap seekhenge ke Linux kis target mein boot hota hai, target files kahan hoti hain aur mukhtalif targets boot ke waqt kin services ko start karte hain.

---

# Background

Purani Linux distributions jaise:

- RHEL 5
- RHEL 6

Runlevels use karti thin.

Lekin modern Linux distributions jaise:

- Rocky Linux 9
- RHEL 9
- CentOS Stream

**systemd targets** use karti hain.

Aap target ko simply **runlevel ka modern replacement** samajh sakte hain.

---

# Common Target Mapping

| Purana Runlevel | systemd Target | Maqsad |
|-----------------|----------------|---------|
| 0 | poweroff.target | System ko shutdown karna |
| 1 | rescue.target | Single-user rescue mode |
| 3 | multi-user.target | Multi-user text mode |
| 5 | graphical.target | Graphical Desktop |
| 6 | reboot.target | System restart karna |

---

# Task 1 – Default Boot Target Maloom Karna

Directory ke andar jayen.

```bash
cd /etc/systemd/system
```

Default target dekhein.

```bash
ls -l default.target
```

Example Output

```text
default.target -> /usr/lib/systemd/system/multi-user.target
```

---

# Discussion

Notice karein ke

```
default.target
```

Aik normal file nahi hai.

Ye aik **symbolic link** hai.

### Sawal

Aap ka system kis target mein boot hone ke liye configured hai?

Jawab

```
multi-user.target
```

---

# multi-user.target ka Matlab

Jab Linux **multi-user.target** mein boot hota hai to:

- Multiple users login kar sakte hain.
- Networking start hoti hai.
- Server ki services start hoti hain.
- Text-based login available hota hai.
- GUI start nahi hoti.

Ye target Linux servers mein sab se zyada use hota hai.

---

# Task 2 – Target Unit Files Dekhna

Systemd unit directory ke andar jayen.

```bash
cd /usr/lib/systemd/system
```

Files dekhein.

```bash
ls -l
```

Observe karein ke yahan bohot sari unit files mojood hain.

---

# Task 3 – Common Targets Dekhna

Rescue target dekhein.

```bash
ls -l rescue.target
```

Multi-user target dekhein.

```bash
ls -l multi-user.target
```

Graphical target dekhein.

```bash
ls -l graphical.target
```

Reboot target dekhein.

```bash
ls -l reboot.target
```

Poweroff target dekhein.

```bash
ls -l poweroff.target
```

---

# Important Targets ko Samajhna

## rescue.target

### Maqsad

- Single-user mode
- Sirf root login kar sakta hai.
- Networking start nahi hoti.
- GUI start nahi hoti.
- System maintenance aur repair ke liye use hota hai.

### Common Use Cases

- Root password reset karna.
- Filesystem repair karna.
- Boot problems fix karna.
- Emergency maintenance.

---

## multi-user.target

### Maqsad

- Multi-user mode
- Text-based login
- Networking enabled
- Server services start hoti hain.
- GUI start nahi hoti.

### Common Use Cases

- Production Linux servers
- Web Servers
- Database Servers
- Application Servers

---

## graphical.target

### Maqsad

Ye target

```
multi-user.target
```

ki tamam services ke sath graphical desktop bhi start karta hai.

### Common Use Cases

- Desktop computers
- Linux Workstations

---

## reboot.target

### Maqsad

System ko restart karna.

---

## poweroff.target

### Maqsad

System ko safely shutdown karna.

---

# Task 4 – Target Dependencies Dekhna

Graphical target ki dependencies dekhein.

```bash
systemctl list-dependencies graphical.target
```

Example

```text
graphical.target

├─display-manager.service

├─multi-user.target

├─NetworkManager.service

├─sshd.service

├─firewalld.service

└─basic.target
```

---

# Dependencies ko Samajhna

Har target akela kaam nahi karta.

Us ke chalne ke liye doosri services aur targets ki zarurat hoti hai.

Misal ke taur par

```
graphical.target
```

Depend karta hai

```
multi-user.target
```

Aur

```
multi-user.target
```

Depend karta hai

- sshd.service
- firewalld.service
- chronyd.service
- NetworkManager.service
- crond.service

Aur bohot si doosri services par.

Isi liye output tree ki shakal mein dikhai deta hai.

---

# Task 5 – Kisi Aur Target ki Dependencies Dekhna

Command chalayein.

```bash
systemctl list-dependencies multi-user.target
```

Output ko graphical.target ke sath compare karein.

### Sawal

Kaunsi service graphical.target mein hai lekin multi-user.target mein nahi?

Hint

```
display-manager.service
```

---

# Dependency Tree ko Samajhna

Output tree ki shakal mein hota hai.

Example

```text
graphical.target

└── multi-user.target

    ├── sshd.service

    ├── firewalld.service

    ├── chronyd.service

    └── basic.target
```

Har branch kisi dependency ko represent karti hai jo target ke successfully start hone ke liye zaroori hoti hai.

---

# Status Symbols ko Samajhna

Jab aap

```bash
systemctl list-dependencies
```

chalate hain to aap ko mukhtalif symbols nazar aate hain.

---

## Green Dot (●)

Matlab

```
Active (running)
```

Service ya target chal raha hai.

---

## White Circle (○)

Matlab

```
Inactive (dead)
```

Service ya target is waqt nahi chal raha.

---

## Red Dot

Matlab

```
Failed
```

Service kisi error ki wajah se start nahi ho saki.

---

# Task 6 – Current Default Target Dekhna

Command chalayein.

```bash
systemctl get-default
```

Example

```text
multi-user.target
```

### Sawal

Kya ye wahi target hai jo symbolic link mein tha?

---

# Task 7 – Sirf Target Files Dekhna

Command chalayein.

```bash
ls *.target
```

Observe karein ke aap ke Linux system mein kitne targets available hain.

---

# RHCSA Challenge 1

Apne system ka default boot target maloom karein.

---

# RHCSA Challenge 2

Check karein ke system kis target mein boot hota hai.

- graphical.target
- multi-user.target

---

# RHCSA Challenge 3

In targets ko locate karein.

- rescue.target
- multi-user.target
- graphical.target
- reboot.target
- poweroff.target

---

# RHCSA Challenge 4

Command chalayein.

```bash
systemctl list-dependencies multi-user.target
```

Kam az kam 5 services ka naam likhein.

---

# RHCSA Challenge 5

Command chalayein.

```bash
systemctl list-dependencies graphical.target
```

Aisi aik dependency dhoondhein jo multi-user.target mein nahi hai.

---

# RHCSA Challenge 6

Command chalayein.

```bash
systemctl get-default
```

Phir verify karein.

```bash
ls -l /etc/systemd/system/default.target
```

### Sawal

Kya dono commands same target show karti hain?

---

# Knowledge Check

1. Modern Linux mein runlevels ki jagah kis cheez ne li hai?

2. Target kya hota hai?

3. Linux servers mein sab se zyada konsa target use hota hai?

4. Graphical desktop konsa target start karta hai?

5. Rescue mode ke liye konsa target use hota hai?

6. Shutdown ke liye konsa target use hota hai?

7. Reboot ke liye konsa target use hota hai?

8. Target unit files kis directory mein hoti hain?

9. default.target ka kya purpose hai?

10. Current default target dekhne ki command kya hai?

11. Kisi target ki dependencies dekhne ki command kya hai?

12. Green dot (●) ka kya matlab hai?

13. White circle (○) ka kya matlab hai?

14. Red dot ka kya matlab hai?

---

# Summary

Is lab ke baad aap confidently:

✅ Runlevels aur systemd targets ka farq samajh sakte hain.

✅ Default boot target identify kar sakte hain.

✅ Target configuration files locate kar sakte hain.

✅ rescue.target, multi-user.target, graphical.target, reboot.target aur poweroff.target ka purpose explain kar sakte hain.

✅ Target dependencies dekh sakte hain.

✅ Dependency tree ko samajh sakte hain.

✅ `systemctl` ke status symbols ko interpret kar sakte hain.

Ye tamam concepts RHCSA exam ke liye bohot important hain aur real-world Linux administration mein rozana istemal hote hain.