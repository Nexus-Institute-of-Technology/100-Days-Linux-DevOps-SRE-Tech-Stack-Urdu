# MODULE 08 – Practice Lab: Password-Based SSH Authentication Ko Prohibit Karna
> **Hands-on Practice Lab – Sirf SSH Key-Based Authentication Allow Karna**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- SSH ki password-based authentication ko disable karna.
- Sirf SSH key-based authentication ko allow karna.
- SSH daemon (`sshd`) ko public key authentication ke liye configure karna.
- `PermitRootLogin` aur `PubkeyAuthentication` parameters ko samajhna.
- Configuration change ke baad SSH service ko reload karna.
- Passwordless SSH login ke liye system ko prepare karna.

---

# 📖 Introduction

By default SSH authentication password ki madad se hoti hai.

Yeh method aam tor par use hota hai, lekin security ke liye sab se behtareen tareeqa nahi hai.

Is se zyada secure method **SSH Public Key Authentication** hai.

SSH Key Authentication ke faide:

- Login ke waqt password transmit nahi hota.
- Authentication cryptographic key pair ki madad se hoti hai.
- Brute-force password attacks ka risk bohot kam ho jata hai.
- Overall system security improve ho jati hai.

Is practice lab mein hum SSH server ko is tarah configure karenge ke sirf **SSH Key-Based Authentication** allow ho.

---

# 🖥️ Lab Scenario

Hamare paas aik RHEL 9 server hai.

Current configuration:

- Direct Root Login pehle hi disable ki ja chuki hai.
- Normal users abhi bhi password use karke login kar sakte hain.

Hamari requirement hai:

```text
SSH Login
      │
      ▼
Sirf Private Key Authentication
```

Matlab sirf wohi users login kar sakein jin ke paas valid SSH Keys mojood hon.

---

# 🔬 Lab 1 – Current SSH Login Verify Karein

Sab se pehle current SSH behavior verify karein.

Root user se login karne ki koshish karein.

```bash
ssh root@server-ip
```

Expected Result:

```text
Permission denied
```

Root login pehle hi disable hai.

Ab normal user ko test karein.

```bash
ssh student@server-ip
```

Password enter karein.

Login successful hona chahiye kyun ke password authentication abhi enabled hai.

---

# 🔬 Lab 2 – Root User Ban Jayein

SSH configuration edit karne ke liye root privileges zaroori hain.

Root account mein switch karein.

```bash
sudo -i
```

Ya:

```bash
su -
```

Verify karein:

```bash
whoami
```

Expected Output:

```text
root
```

---

# 🔬 Lab 3 – SSH Configuration File Open Karein

SSH daemon ki configuration file edit karein.

```bash
vim /etc/ssh/sshd_config
```

---

# 🔬 Lab 4 – Root Login Configure Karein

Neeche wala parameter dhoondein:

```text
PermitRootLogin
```

Aap ko kuch is tarah nazar aa sakta hai:

```text
#PermitRootLogin prohibit-password
```

Is line ko uncomment karein.

Configuration kuch is tarah honi chahiye:

```text
PermitRootLogin prohibit-password
```

### Iska Matlab

Is setting ka matlab hai:

Root user sirf **SSH Keys** ke zariye login kar sakta hai.

Root password se login karna prohibit (disable) kar diya gaya hai.

---

# 🔬 Lab 5 – Public Key Authentication Enable Karein

Ab neeche wala parameter dhoondein:

```text
#PubkeyAuthentication yes
```

Isay bhi uncomment karein.

Final configuration:

```text
PubkeyAuthentication yes
```

Ye SSH Public Key Authentication ko enable karta hai.

---

# Configuration Ki Samajh

| Parameter | Matlab |
|-----------|---------|
| `PermitRootLogin prohibit-password` | Root sirf SSH Keys se login karega |
| `PubkeyAuthentication yes` | SSH Key Authentication enable karega |

---

# 🔬 Lab 6 – Configuration Save Karein

Configuration save karein aur editor se bahar aa jayein.

---

# 🔬 Lab 7 – Configuration Validate Karein

SSH reload karne se pehle configuration syntax verify karein.

```bash
sshd -t
```

Agar koi output na aaye to configuration bilkul theek hai.

---

# 🔬 Lab 8 – SSH Service Reload Karein

SSH daemon ko reload karein.

```bash
systemctl reload sshd
```

Ya:

```bash
systemctl reload sshd.service
```

Ab SSH daemon nayi configuration use karega.

---

# 🔬 Lab 9 – Effective Configuration Verify Karein

Effective SSH configuration check karein.

```bash
sshd -T | grep -Ei "permitrootlogin|pubkeyauthentication"
```

Expected Output:

```text
permitrootlogin prohibit-password
pubkeyauthentication yes
```

---

# 🔬 Lab 10 – SSH Key Authentication Ke Liye Prepare Karein

Is stage par:

- Root password authentication disable ho chuki hai.
- Public Key Authentication enable ho chuki hai.

Lekin passwordless login tabhi kaam karega jab users apni SSH key pair generate karenge.

Ye aglay practice lab mein cover kiya jayega.

---

# Configuration Flow

```text
SSH Client
      │
      ▼
Private Key
      │
Encrypted Authentication
      │
      ▼
SSH Server
      │
Authorized Public Key
      │
      ▼
Login Successful
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Current Root Login configuration verify karein.

```bash
grep PermitRootLogin /etc/ssh/sshd_config
```

---

## Exercise 2

Public Key Authentication verify karein.

```bash
grep PubkeyAuthentication /etc/ssh/sshd_config
```

---

## Exercise 3

SSH configuration validate karein.

```bash
sshd -t
```

---

## Exercise 4

SSH daemon reload karein.

```bash
systemctl reload sshd
```

---

## Exercise 5

Effective SSH configuration verify karein.

```bash
sshd -T | grep -Ei "permitrootlogin|pubkeyauthentication"
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

SSH service reload nahi ho rahi.

Configuration check karein.

```bash
sshd -t
```

Syntax errors ko pehle theek karein.

---

### Scenario 2

Public Key Authentication kaam nahi kar rahi.

Verify karein:

```bash
PubkeyAuthentication yes
```

Effective configuration check karein.

```bash
sshd -T | grep pubkeyauthentication
```

---

### Scenario 3

Root password se abhi bhi login ho raha hai.

Verify karein.

```bash
sshd -T | grep permitrootlogin
```

Expected Output:

```text
permitrootlogin prohibit-password
```

---

### Scenario 4

Configuration change ke baad SSH login fail ho gaya.

Logs check karein.

```bash
journalctl -u sshd
```

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `vim /etc/ssh/sshd_config` | SSH configuration edit kare |
| `PermitRootLogin prohibit-password` | Root sirf SSH Keys se login kare |
| `PubkeyAuthentication yes` | SSH Key Authentication enable kare |
| `sshd -t` | SSH configuration validate kare |
| `systemctl reload sshd` | SSH configuration reload kare |
| `sshd -T` | Effective SSH configuration dikhaye |

---

# 📖 Key Takeaways

- Password-based authentication, SSH Keys ke muqable mein kam secure hoti hai.
- `PermitRootLogin prohibit-password` Root ke liye password authentication disable karta hai aur sirf SSH Keys allow karta hai.
- `PubkeyAuthentication yes` SSH Key Authentication enable karta hai.
- Configuration edit karne ke baad hamesha `sshd -t` chalayein.
- Configuration update ke baad SSH daemon reload karein.
- Passwordless login ke liye SSH Keys generate aur install karna zaroori hai.

---

# 💡 Yaad Rakhein

> **SSH Keys ko digital ID Card ki tarah samjhein.**
>
> - Password guess ya chori ho sakta hai.
> - Private Key unique aur bohot zyada secure hoti hai.
> - Server aap ki identity ko Public Key ki madad se verify karta hai, password transmit kiye baghair.
>
> **Golden Rule:**
>
> ```text
> SSH Client
>      │
> Private Key
>      │
>      ▼
> Public Key Verification
>      │
>      ▼
> Secure Login
> ```
>
> **Jahan bhi mumkin ho, password authentication ki bajaye SSH Key-Based Authentication use karein. Yeh Linux security ki best practice hai.**