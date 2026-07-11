# MODULE 08 – Practice Lab: SSH Private Key-Based Login
> **Hands-on Practice Lab – Root User Ke Liye SSH Private Key Authentication Configure Karna**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge:

- SSH Public aur Private Key pair generate karna.
- `ssh-keygen` command ki madad se RSA key pair create karna.
- Private Key ko `.pem` extension dena.
- `.ssh` directory configure karna.
- `authorized_keys` file create karna.
- Public Key ko `authorized_keys` mein copy karna.
- SSH service ko reload karna.
- MobaXterm ya PuTTY mein Private Key configure karna.
- Password ke baghair Linux server par login karna.

---

# 📖 Introduction

SSH Key-Based Authentication Linux systems mein login karne ka sab se secure tareeqa hai.

Har dafa password type karne ki bajaye SSH do cryptographic keys use karta hai.

Ye do keys hoti hain:

- **Private Key** – Jo client machine par secure rakhi jati hai.
- **Public Key** – Jo Linux server par store ki jati hai.

Authentication ke dauran:

1. Client apni Private Key ki ownership prove karta hai.
2. Server usay apni stored Public Key se verify karta hai.
3. Agar dono match ho jayein to password ke baghair login ho jata hai.

Is practice lab mein hum Root user ke liye SSH Key-Based Authentication configure karenge.

---

# 🖥️ Lab Scenario

Hamare paas aik Linux server hai.

Hamara maqsad hai:

```text
Windows Client
        │
        │  Private Key (.pem)
        ▼
Linux Server
        │
Authorized Public Key
        ▼
Passwordless Root Login
```

---

# 🔬 Lab 1 – Root Home Directory Mein Jayein

Agar zarurat ho to Root account mein switch karein.

```bash
sudo -i
```

Root ki Home Directory mein jayein.

```bash
cd /root
```

Current location verify karein.

```bash
pwd
```

Expected Output:

```text
/root
```

---

# 🔬 Lab 2 – Keys Directory Create Karein

Keys ko store karne ke liye aik directory banayein.

```bash
mkdir keys
```

Aap koi bhi naam use kar sakte hain.

Directory ke andar jayein.

```bash
cd keys
```

---

# 🔬 Lab 3 – SSH Key Pair Generate Karein

RSA SSH Key Pair generate karein.

```bash
ssh-keygen -t rsa
```

Agar chahein to custom filename bhi de sakte hain.

Example:

```bash
ssh-keygen -t rsa -f root_key
```

Is se do files create hongi.

```text
root_key
root_key.pub
```

---

# Generated Files Ko Samjhein

| File | Kaam |
|------|------|
| `root_key` | Private Key |
| `root_key.pub` | Public Key |

Private Key hamesha secret aur secure rakhein.

Public Key ko server par copy kiya jata hai.

---

# 🔬 Lab 4 – Private Key Rename Karein

Private Key ko `.pem` extension dein.

Example:

```bash
mv root_key root_key.pem
```

`.pem` extension ko bohot se SSH clients support karte hain, jaise:

- MobaXterm
- PuTTY
- AWS EC2
- OpenSSH Clients

---

# 🔬 Lab 5 – `.ssh` Directory Create Karein

Root Home Directory mein wapas aayein.

```bash
cd /root
```

`.ssh` directory create karein.

```bash
mkdir .ssh
```

Is directory ki permissions set karein.

```bash
chmod 700 .ssh
```

---

# Permission 700 Kyun?

Permission **700** ka matlab hai:

- Owner: Read, Write, Execute
- Group: No Access
- Others: No Access

SSH secure permissions require karta hai.

---

# 🔬 Lab 6 – `authorized_keys` File Create Karein

`.ssh` directory ke andar jayein.

```bash
cd .ssh
```

`authorized_keys` file create karein.

```bash
touch authorized_keys
```

Ye file un tamam Public Keys ko store karti hai jinhein login ki permission hoti hai.

---

# 🔬 Lab 7 – Public Key Copy Karein

Keys directory mein wapas aayein.

```bash
cd /root/keys
```

Public Key ko `authorized_keys` file mein copy karein.

Example:

```bash
cat root_key.pub >> /root/.ssh/authorized_keys
```

Verify karein.

```bash
cat /root/.ssh/authorized_keys
```

Ab Public Key ka content is file mein nazar aana chahiye.

---

# Authentication Flow

```text
Private Key
      │
      ▼
Client
      │
Encrypted Authentication
      │
      ▼
Server
      │
authorized_keys
      │
      ▼
Login Successful
```

---

# 🔬 Lab 8 – SSH Service Reload Karein

SSH daemon reload karein.

```bash
systemctl reload sshd
```

Ab SSH nayi configuration use karega.

---

# 🔬 Lab 9 – Private Key Windows Machine Par Copy Karein

Private Key ko Windows machine par copy karein.

Aap ye kisi bhi tareeqay se kar sakte hain.

Jaise:

- WinSCP
- WinZip
- SCP
- Notepad mein Copy/Paste

File ko save karein:

```text
root_key.pem
```

Example folder:

```text
E:\Keys\
```

---

# 🔬 Lab 10 – MobaXterm Configure Karein

MobaXterm open karein.

Nayi SSH Session create karein.

Server:

```text
Server IP Address
```

Username:

```text
root
```

Ab:

```text
Advanced SSH Settings
```

Open karein.

Option select karein:

```text
Use Private Key
```

Aur browse karke select karein:

```text
root_key.pem
```

Phir **OK** par click karein.

---

# Expected Result

Ab password poochne ki bajaye,

Server Private Key ki madad se authentication karega.

Login successful ho jayega.

---

# Password Login vs SSH Key Login

## Password Authentication

```text
Client
    │
Password
    ▼
Server
```

---

## SSH Key Authentication

```text
Client
    │
Private Key
    ▼
Encrypted Authentication
    ▼
Server
    │
authorized_keys
```

---

# 🧪 Practice Exercises

---

## Exercise 1

Nayi RSA Key Pair generate karein.

```bash
ssh-keygen -t rsa
```

---

## Exercise 2

`.ssh` directory create karein.

```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
```

---

## Exercise 3

`authorized_keys` file create karein.

```bash
touch ~/.ssh/authorized_keys
```

---

## Exercise 4

Public Key append karein.

```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

---

## Exercise 5

SSH service reload karein.

```bash
systemctl reload sshd
```

---

## Exercise 6

Apni Private Key use karke SSH client se login karein.

---

# 🔧 Troubleshooting Scenarios

### Scenario 1

SSH abhi bhi password maang rahi hai.

Verify karein ke Public Key mojood hai.

```bash
cat ~/.ssh/authorized_keys
```

---

### Scenario 2

`.ssh` directory ki permissions ghalat hain.

Theek karein.

```bash
chmod 700 ~/.ssh
```

---

### Scenario 3

`authorized_keys` ki permissions ghalat hain.

Theek karein.

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

### Scenario 4

SSH nayi configuration use nahi kar rahi.

Service reload karein.

```bash
systemctl reload sshd
```

---

### Scenario 5

MobaXterm mein Private Key select nahi hui.

Open karein:

```text
Advanced SSH Settings
```

Option enable karein:

```text
Use Private Key
```

Aur sahi `.pem` file select karein.

---

# 📌 Quick Revision

| Command | Kaam |
|----------|------|
| `ssh-keygen -t rsa` | RSA SSH Key Pair Generate Kare |
| `mkdir .ssh` | SSH Directory Create Kare |
| `chmod 700 .ssh` | `.ssh` Directory Secure Kare |
| `touch authorized_keys` | Authorized Keys File Create Kare |
| `cat key.pub >> authorized_keys` | Public Key Authorized Keys Mein Copy Kare |
| `systemctl reload sshd` | SSH Service Reload Kare |
| `chmod 600 authorized_keys` | Authorized Keys File Secure Kare |

---

# 📖 Key Takeaways

- SSH Key Authentication password se zyada secure hoti hai.
- Har Key Pair mein aik Private Key aur aik matching Public Key hoti hai.
- **Private Key kabhi kisi ke saath share nahi karni chahiye.**
- **Public Key** ko `authorized_keys` file mein copy kiya jata hai.
- `.ssh` directory ki permission **700** honi chahiye.
- `authorized_keys` file ki permission **600** honi chahiye.
- Configuration complete hone ke baad password ke baghair login mumkin ho jata hai.

---

# 💡 Yaad Rakhein

> **SSH Authentication ko office ki security system ki tarah samjhein.**
>
> - **Private Key** aap ka personal security card hai.
> - **Public Key** office ke security desk par approved cards ki list hai.
> - Jab aap login karte hain to security guard aap ke card ko approved list se compare karta hai.
> - Agar dono match kar jayein to darwaza khul jata hai.
>
> **Golden Rule:**
>
> ```text
> Private Key
>      │
>      ▼
> Kabhi Share Na Karein
>      │
>      ▼
> Public Key
>      │
>      ▼
> authorized_keys
>      │
>      ▼
> Secure Passwordless Login
> ```
>
> **Apni Private Key ko hamesha secure rakhein. Jis ke paas Private Key hogi, woh aap ke servers tak access hasil kar sakta hai.**