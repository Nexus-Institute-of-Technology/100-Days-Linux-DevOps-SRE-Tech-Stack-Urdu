# MODULE 08 – Practice Lab: Prohibit Root User Login
> **Hands-on Practice Lab – Disable Direct Root SSH Login**

---

# 🎯 Lab Objective

Is practice lab mein hum seekhenge:

- Kaise direct **root SSH login** ko disable karna hai.
- `sshd_config` file ko edit karna.
- `PermitRootLogin` parameter ka use samajhna.
- SSH service ko reload karna.
- Configuration change ko verify karna.
- Security best practices ko follow karna.

---

# 📖 Introduction

Production Linux environments mein **direct root login** allow karna recommend nahi kiya jata.

Behtar tareeqa yeh hai ke:

- Pehle apne **normal user account** se login karein.
- Agar administrative kaam karna ho to `sudo` ya `su -` use karein.
- Is tarah auditing aur accountability bhi maintain rehti hai.

Is lab mein hum **PermitRootLogin** parameter ko use karke direct root SSH login ko prohibit karenge.

---

# 🖥️ Lab Scenario

Hamare paas ek Linux server hai.

Current configuration mein root user SSH ke zariye login kar sakta hai.

Hamari requirement hai:

```text
Root SSH Login
        │
        ▼
Blocked
```

Lekin normal users SSH ke zariye login kar sakein.

---

# 🔬 Lab 1 – Verify Current Root Login

Sab se pehle verify karein ke root login currently allowed hai.

SSH command:

```bash
ssh root@192.168.56.102
```

Password enter karein.

Agar login successful ho jaye to iska matlab hai:

```text
PermitRootLogin yes
```

Ya koi aisi configuration hai jo root login allow kar rahi hai.

---

# 🔬 Lab 2 – Login as a Normal User

Ab system mein ek normal user se login karein.

Example:

```bash
ssh student@192.168.56.102
```

Ya agar console par hain:

```bash
login
```

---

# 🔬 Lab 3 – Elevate Privileges

SSH configuration sirf root edit kar sakta hai.

Root privileges hasil karein.

Using `su`:

```bash
su -
```

Ya:

```bash
sudo -i
```

Verify:

```bash
whoami
```

Expected Output:

```text
root
```

---

# 🔬 Lab 4 – Edit SSH Configuration

SSH daemon configuration file open karein.

```bash
vim /etc/ssh/sshd_config
```

Search karein:

```text
PermitRootLogin
```

Agar line commented ho to uncomment karein.

Current value:

```text
PermitRootLogin yes
```

Isay change karein:

```text
PermitRootLogin no
```

Save karein aur editor se bahar aa jayein.

---

# 🔬 Lab 5 – Verify Configuration

Configuration verify karein.

```bash
grep PermitRootLogin /etc/ssh/sshd_config
```

Expected Output:

```text
PermitRootLogin no
```

---

# 🔬 Lab 6 – Validate Configuration

Restart ya reload se pehle syntax verify karna best practice hai.

```bash
sshd -t
```

Agar koi output na aaye to configuration valid hai.

---

# 🔬 Lab 7 – Reload SSH Service

Configuration reload karein.

```bash
systemctl reload sshd
```

Ya kuch systems par:

```bash
systemctl reload sshd.service
```

Reload ka faida yeh hai ke:

- Existing SSH sessions disconnect nahi hoti.
- Sirf configuration dobara read hoti hai.

---

# Reload vs Restart

| Command | Purpose |
|----------|---------|
| `systemctl reload sshd` | Configuration dobara load karta hai |
| `systemctl restart sshd` | SSH service ko completely restart karta hai |

Production systems mein agar possible ho to **reload** prefer kiya jata hai.

---

# 🔬 Lab 8 – Test Root Login Again

Ab dobara root login test karein.

```bash
ssh root@192.168.56.102
```

Password enter karein.

Expected Result:

```text
Permission denied
```

Ya

```text
Access denied
```

Root user ab SSH ke zariye login nahi kar sakta.

---

# 🔬 Lab 9 – Verify Normal User Login

Ab normal user login test karein.

```bash
ssh student@192.168.56.102
```

Login successful hona chahiye.

Uske baad agar administrative access chahiye:

```bash
sudo -i
```

Ya:

```bash
su -
```

---

# 🔬 Lab 10 – Check Effective SSH Configuration

Current effective SSH configuration check karein.

```bash
sshd -T | grep permitrootlogin
```

Expected Output:

```text
permitrootlogin no
```

---

# Practice Exercises

---

## Exercise 1

Current root login setting check karein.

```bash
grep PermitRootLogin /etc/ssh/sshd_config
```

---

## Exercise 2

Configuration syntax verify karein.

```bash
sshd -t
```

---

## Exercise 3

SSH service reload karein.

```bash
systemctl reload sshd
```

---

## Exercise 4

Root login test karein.

```bash
ssh root@server-ip
```

---

## Exercise 5

Normal user login test karein.

```bash
ssh student@server-ip
```

---

## Exercise 6

Effective SSH configuration verify karein.

```bash
sshd -T | grep permitrootlogin
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

Root login ab bhi allow ho raha hai.

Check:

```bash
sshd -T | grep permitrootlogin
```

Agar output:

```text
permitrootlogin yes
```

To configuration dobara check karein.

---

### Scenario 2

Configuration error aa raha hai.

Run:

```bash
sshd -t
```

Syntax error ko fix karein.

---

### Scenario 3

Reload fail ho gaya.

Status check karein.

```bash
systemctl status sshd
```

Logs check karein.

```bash
journalctl -u sshd
```

---

### Scenario 4

Normal user bhi login nahi kar pa raha.

Verify:

```bash
AllowUsers
```

Aur:

```bash
PasswordAuthentication
```

SSH configuration mein.

---

# 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `vim /etc/ssh/sshd_config` | SSH configuration edit kare |
| `PermitRootLogin no` | Root SSH login disable kare |
| `grep PermitRootLogin` | Current setting verify kare |
| `sshd -t` | Configuration syntax check kare |
| `systemctl reload sshd` | SSH configuration reload kare |
| `sshd -T` | Effective SSH configuration dikhaye |
| `ssh root@server` | Root login test kare |

---

# 📖 Key Takeaways

- Direct root SSH login production systems mein avoid karna chahiye.
- `PermitRootLogin no` root login ko disable karta hai.
- Configuration edit karne ke baad `sshd -t` zaroor chalayein.
- Service ko reload karna restart se behtar hota hai jab sirf configuration update hui ho.
- Normal user se login karein aur zarurat par `sudo` ya `su -` use karein.
- Yeh approach security aur accountability dono improve karti hai.

---

# 💡 Remember

> **Root account ko office ki Master Key samjhein.**
>
> - Har kisi ko Master Key dena secure nahi hota.
> - Har administrator apni personal key (normal user account) se office mein enter karta hai.
> - Zarurat par hi Master Key (`sudo` ya `su -`) use karta hai.
>
> **Golden Rule:**
>
> ```text
> SSH Login
>      │
>      ▼
> Normal User
>      │
>      ▼
> sudo / su -
>      │
>      ▼
> Administrative Tasks
> ```
>
> **Direct Root SSH Login ko hamesha disable rakhna security ki best practice hai.**