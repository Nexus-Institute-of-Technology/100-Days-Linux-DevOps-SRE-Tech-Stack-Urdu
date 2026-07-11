# MODULE 08 – SSH Key-Based Authentication
> **Public Aur Private Keys Ke Zariye Secure Passwordless SSH Login Configure Karna (Roman Urdu)**

---

# 🎯 Learning Objectives

Is lesson mein aap seekhenge:

- SSH Key-Based Authentication kya hai.
- Passwordless SSH authentication kis tarah kaam karti hai.
- Public Key aur Private Key ke darmiyan farq.
- SSH keys kahan store hoti hain.
- Private Key ko secret rakhna kyun zaroori hai.
- Public Key remote server par kaise install hoti hai.
- `authorized_keys` file ka kya kaam hai.
- SSH challenge-response authentication process kis tarah kaam karta hai.
- Compromised Private Key dangerous kyun hoti hai.
- SSH keys ke important security best practices.

---

# 📖 Introduction

Default tor par SSH user ko password ke zariye authenticate kar sakta hai.

Example:

```bash
ssh user1@serverB
```

Remote server ye password maang sakta hai:

```text
user1@serverB's password:
```

Lekin SSH ko is tarah bhi configure kiya ja sakta hai ke user account password ke bajaye cryptographic keys se authenticate ho.

Isay kehte hain:

> **SSH Key-Based Authentication**

Ye aam tor par use hoti hai:

- Secure remote administration
- Automation
- Backup jobs
- Configuration management
- DevOps pipelines
- Server-to-server communication

---

# 1. SSH Key-Based Authentication Kya Hai?

SSH Key-Based Authentication user ko cryptographic key pair ke zariye remote server par login karne ki ijazat deti hai.

Key pair mein do keys hoti hain:

1. **Private Key**
2. **Public Key**

Private Key client system par rehti hai.

Public Key remote server par copy ki jati hai.

---

# Key-Based Authentication Layout

```text
Server A – SSH Client
User: user1
│
├── Private Key
│   └── Secret rehni chahiye
│
└── Public Key
    └── Server B par copy hoti hai
```

```text
Server B – SSH Server
User: user1
│
└── ~/.ssh/authorized_keys
    └── user1 ki Public Key mojood hoti hai
```

---

# 2. Practice Lab Environment

Suppose do Linux servers hain:

| Server | Role | User |
|--------|------|------|
| Server A | SSH Client | `user1` |
| Server B | SSH Server | `user1` |

User `user1` dono servers par mojood hai.

Requirement ye hai:

> Server A par mojood `user1`, Server B par remote account password type kiye baghair login kar sake.

---

# 3. Password Authentication

SSH keys ke baghair normal login process:

```bash
ssh user1@serverB
```

Phir Server B poochta hai:

```text
user1@serverB's password:
```

User ko Server B par configured `user1` ka password enter karna hota hai.

---

# Password Authentication Flow

```text
User SSH Command Chalata Hai
        │
        ▼
Server Password Maangta Hai
        │
        ▼
User Password Enter Karta Hai
        │
        ▼
Server Password Verify Karta Hai
        │
        ▼
Login Allow Ya Deny Hota Hai
```

---

# 4. Key-Based Authentication

Key-Based Authentication mein:

- Server A par Private Key hoti hai.
- Server B par matching Public Key hoti hai.
- User ko remote account password bhejne ki zaroorat nahi hoti.
- Server verify karta hai ke client ke paas matching Private Key mojood hai.

---

# Key-Based Authentication Flow

```text
Server A
Private Key
    │
    │ SSH Connection Request
    ▼
Server B
Public Key in authorized_keys
    │
    │ Cryptographic Verification
    ▼
Identity Confirm Hoti Hai
    │
    ▼
SSH Login Allow Hota Hai
```

---

# 5. Public Key Aur Private Key

## Private Key

Private Key:

- Client par rehni chahiye.
- Protect honi chahiye.
- Kabhi share nahi karni chahiye.
- User ki identity prove karti hai.
- Passphrase se protect ki ja sakti hai.

Example files:

```text
~/.ssh/id_ed25519
```

ya:

```text
~/.ssh/id_rsa
```

---

## Public Key

Public Key:

- Remote servers par copy ki ja sakti hai.
- Secret nahi hoti.
- Remote user ki `authorized_keys` file mein rakhi jati hai.
- Sirf matching Private Key ke saath kaam karti hai.

Example files:

```text
~/.ssh/id_ed25519.pub
```

ya:

```text
~/.ssh/id_rsa.pub
```

---

# Encryption Ke Bare Mein Important Correction

Aam tor par kaha jata hai:

- Public Key encrypt karti hai.
- Private Key decrypt karti hai.

Lekin SSH user authentication ko zyada accurately **cryptographic proof** kaha jata hai.

SSH server sirf ek normal secret message nahi bhejta jo client decrypt kare.

Asal process:

1. Client Public Key offer karta hai.
2. Server check karta hai ke Public Key authorized hai ya nahi.
3. Client matching Private Key ki possession prove karta hai by signing authentication data.
4. Server Public Key se signature verify karta hai.
5. Private Key network par kabhi send nahi hoti.

---

# 6. SSH Keys Kahan Store Hoti Hain?

Server A par `user1` ke taur par keys aam tor par is jagah store hoti hain:

```text
/home/user1/.ssh/
```

Shortcut:

```text
~/.ssh/
```

Typical files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

---

# 7. `authorized_keys` File

Server B par Public Key is file mein store hoti hai:

```text
/home/user1/.ssh/authorized_keys
```

Shortcut:

```text
~/.ssh/authorized_keys
```

Is file mein woh Public Keys hoti hain jinhein us user ke taur par login ki permission hoti hai.

---

# Important Rule

Public Key correct remote account ke andar install honi chahiye.

Misal ke taur par, agar login karna hai:

```text
user1
```

ke taur par, to key is path mein honi chahiye:

```text
/home/user1/.ssh/authorized_keys
```

Ye kisi aur user ke home directory mein nahi honi chahiye.

---

# 8. SSH Key Pair Generate Karein

Server A par `user1` ke taur par login karein.

Verify karein:

```bash
whoami
```

Expected Output:

```text
user1
```

ED25519 key pair generate karein:

```bash
ssh-keygen -t ed25519
```

---

# ED25519 Kyun Use Karein?

ED25519 aam tor par recommended hai kyun ke ye provide karta hai:

- Strong security
- Chhoti key size
- Fast authentication
- Achhi performance
- Modern cryptography

---

# Key Generation Prompt

Aap ko ye prompt mil sakta hai:

```text
Enter file in which to save the key (/home/user1/.ssh/id_ed25519):
```

Default path accept karne ke liye **Enter** press karein.

Phir:

```text
Enter passphrase (empty for no passphrase):
```

Aap:

- Better security ke liye strong passphrase enter kar sakte hain.
- Fully unattended automation ke liye empty chhor sakte hain.

---

# Security Recommendation

Interactive administrator accounts ke liye passphrase use karein.

Automation ke liye carefully consider karein:

- Dedicated service account
- Restricted key permissions
- Command restrictions
- Source-address restrictions
- Secure secret storage

---

# 9. `ssh-keygen` Se Kaun Si Files Create Hoti Hain?

Generation ke baad verify karein:

```bash
ls -la ~/.ssh
```

Expected files:

```text
id_ed25519
id_ed25519.pub
```

---

# Key Files Ka Matlab

| File | Kaam |
|------|------|
| `id_ed25519` | Private Key |
| `id_ed25519.pub` | Public Key |

---

# 10. Private Key Ko Protect Karein

Permissions check karein:

```bash
ls -l ~/.ssh/id_ed25519
```

Typical secure permissions:

```text
-rw-------.
```

Zaroorat ho to set karein:

```bash
chmod 600 ~/.ssh/id_ed25519
```

SSH directory ko secure karein:

```bash
chmod 700 ~/.ssh
```

---

# ⚠️ Private Key Kabhi Share Na Karein

Ye kaam na karein:

- Private Key email na karein.
- Random servers par copy na karein.
- Public repository mein store na karein.
- Tickets ya chat mein paste na karein.
- Kisi aur user ko na dein.
- Shared folder mein na rakhein.

Jis shakhs ke paas Private Key aa jaye, woh un systems tak access hasil kar sakta hai jahan matching Public Key authorized ho.

---

# 11. Public Key Dekhein

Run karein:

```bash
cat ~/.ssh/id_ed25519.pub
```

Example:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... user1@serverA
```

Public Key ko safely remote server par copy kiya ja sakta hai.

---

# 12. `ssh-copy-id` Se Public Key Copy Karein

Recommended method:

```bash
ssh-copy-id user1@serverB
```

IP address ke saath:

```bash
ssh-copy-id user1@192.168.1.67
```

Remote server ek aakhri dafa account password maangega.

Successful installation ke baad Public Key add ho jayegi:

```text
/home/user1/.ssh/authorized_keys
```

---

# Specific Public Key Specify Karein

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user1@serverB
```

---

# 13. Manual Public-Key Installation

Agar `ssh-copy-id` available na ho to:

```bash
cat ~/.ssh/id_ed25519.pub | ssh user1@serverB 'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

Ye command:

1. Local Public Key read karti hai.
2. Server B se connect karti hai.
3. Remote `.ssh` directory create karti hai agar zaroorat ho.
4. Public Key ko `authorized_keys` mein append karti hai.

---

# 14. Remote Public Key Verify Karein

Server B par login karein:

```bash
ssh user1@serverB
```

Phir check karein:

```bash
cat ~/.ssh/authorized_keys
```

Aap ko Server A se copied Public Key nazar aani chahiye.

---

# 15. Correct Remote Permissions

Server B par:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Ownership verify karein:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

Files ka owner hona chahiye:

```text
user1:user1
```

Agar zaroorat ho to ownership fix karein:

```bash
chown -R user1:user1 /home/user1/.ssh
```

---

# 16. Passwordless Login Test Karein

Server A se:

```bash
ssh user1@serverB
```

Agar sab sahi configure hai:

- Remote account password nahi maanga jayega.
- User `user1` ke taur par login hoga.
- Remote shell open ho jayegi.

Verify karein:

```bash
whoami
hostname
pwd
```

---

# Passwordless Authentication Result

```text
Server A
user1
   │
   │ ssh user1@serverB
   ▼
Server B
user1
   │
   └── Remote account password nahi maanga jata
```

---

# 17. Background Mein Kya Hota Hai?

Simplified process:

1. `user1` Server A se SSH connection start karta hai.
2. Server B apni Host Key present karta hai.
3. Server A, Server B ki identity verify karta hai.
4. Server A user ki Public Key identity offer karta hai.
5. Server B `~/.ssh/authorized_keys` check karta hai.
6. Agar key authorized ho to Server B client se matching Private Key ki possession prove karne ko kehta hai.
7. Server A Private Key se authentication data sign karta hai.
8. Server B Public Key se signature verify karta hai.
9. Authentication successful hoti hai.
10. Encrypted SSH session open ho jati hai.

---

# Authentication Flow

```text
Client Public Key Offer Karta Hai
        │
        ▼
Server authorized_keys Check Karta Hai
        │
        ├── Key Nahi Milti ──► Authentication Reject
        │
        └── Key Milti Hai
                │
                ▼
Client Private-Key Possession Prove Karta Hai
                │
                ▼
Server Signature Verify Karta Hai
                │
                ▼
Authentication Successful
```

---

# 18. Private Key Client Se Bahar Nahi Jati

Important security principle:

> **Normal authentication ke dauran Private Key SSH server par copy nahi hoti.**

Server sirf Public Key store karta hai.

Client locally Private Key use karke apni identity prove karta hai.

---

# 19. Ek Key Multiple Servers Par Authorized Ho Sakti Hai

Same Public Key ko multiple servers par copy kiya ja sakta hai:

```text
Server B
Server C
Server D
```

Is se matching Private Key in sab servers par authenticate kar sakti hai.

Example:

```bash
ssh-copy-id user1@serverB
ssh-copy-id user1@serverC
ssh-copy-id user1@serverD
```

---

# Security Impact

Agar Private Key compromise ho jaye to attacker un tamam servers tak access hasil kar sakta hai jahan matching Public Key authorized hai.

Isi liye Private Key ko secure rakhna bohot zaroori hai.

---

# 20. Private-Key Compromise

Suppose attacker ke paas aa jaye:

```text
~/.ssh/id_ed25519
```

Woh un tamam systems par login ki koshish kar sakta hai jahan matching Public Key mojood ho:

```text
~/.ssh/authorized_keys
```

Possible affected servers:

```text
Server B
Server C
Server D
```

---

# Agar Private Key Compromise Ho Jaye To Kya Karein?

Foran:

1. Har remote `authorized_keys` file se matching Public Key remove karein.
2. Naya key pair generate karein.
3. Nayi Public Key approved servers par copy karein.
4. Investigate karein key expose kaise hui.
5. SSH authentication logs review karein.
6. Zaroorat ho to doosre credentials bhi rotate karein.

---

# 21. Public-Key Fingerprint Identify Karein

Local Public-Key fingerprint display karein:

```bash
ssh-keygen -lf ~/.ssh/id_ed25519.pub
```

Example:

```text
256 SHA256:xxxxxxxxxxxxxxxx user1@serverA (ED25519)
```

Fingerprints full key dikhaye baghair key identify karne mein madad karti hain.

---

# 22. Check Karein SSH Kaun Si Key Use Kar Raha Hai

Verbose mode use karein:

```bash
ssh -v user1@serverB
```

Zyada detail ke liye:

```bash
ssh -vv user1@serverB
```

In messages ko dekhein:

```text
Offering public key
Server accepts key
Authentication succeeded
```

---

# 23. Specific Private Key Use Karein

Agar multiple keys hon to:

```bash
ssh -i ~/.ssh/id_ed25519 user1@serverB
```

Example:

```bash
ssh -i ~/.ssh/project_key user1@192.168.1.67
```

---

# 24. SSH Client Configure Karein

Edit karein:

```bash
vim ~/.ssh/config
```

Add karein:

```text
Host serverB
    HostName 192.168.1.67
    User user1
    IdentityFile ~/.ssh/id_ed25519
```

File secure karein:

```bash
chmod 600 ~/.ssh/config
```

Ab connect karein:

```bash
ssh serverB
```

---

# 25. Key Passphrase Aur Account Password Mein Farq

Dono mukhtalif hain.

| Credential | Kaam |
|------------|------|
| Remote account password | Remote operating-system account authenticate karta hai |
| Private-Key passphrase | Local Private Key file ko protect karti hai |

Agar key par passphrase ho to SSH pooch sakta hai:

```text
Enter passphrase for key '/home/user1/.ssh/id_ed25519':
```

Ye remote user ka account password nahi hota.

---

# 26. `ssh-agent` Use Karein

`ssh-agent` decrypted Private Key ko temporary memory mein hold kar sakta hai.

Agent start karein:

```bash
eval "$(ssh-agent -s)"
```

Key add karein:

```bash
ssh-add ~/.ssh/id_ed25519
```

Loaded keys list karein:

```bash
ssh-add -l
```

Key add hone ke baad SSH us session mein baar baar passphrase maange baghair key use kar sakta hai.

---

# 27. SSH Server Configuration

SSH server configuration file:

```text
/etc/ssh/sshd_config
```

Important setting:

```text
PubkeyAuthentication yes
```

Bohot se systems par ye default tor par enabled hoti hai.

Effective configuration check karein:

```bash
sudo sshd -T | grep -i pubkeyauthentication
```

Expected Output:

```text
pubkeyauthentication yes
```

---

# 28. SSH Server Configuration Validate Karein

`sshd` restart karne se pehle:

```bash
sudo sshd -t
```

Agar koi output na aaye to syntax aam tor par valid hai.

Phir zaroorat ho to restart karein:

```bash
sudo systemctl restart sshd
```

---

# 29. Password Authentication Disable Karna

Key-Based Authentication successfully test karne ke baad password authentication disable ki ja sakti hai.

Edit karein:

```bash
sudo vim /etc/ssh/sshd_config
```

Set karein:

```text
PasswordAuthentication no
```

Validate karein:

```bash
sudo sshd -t
```

Restart karein:

```bash
sudo systemctl restart sshd
```

---

# ⚠️ Khud Ko Lock Out Na Karein

Password authentication disable karne se pehle:

- Current SSH session open rakhein.
- Doosri terminal open karein.
- Key-Based Authentication test karein.
- `sudo` ya admin access confirm karein.
- Confirm karein correct key use ho rahi hai.
- Firewall aur SSH service status verify karein.

Naya login successful test hone se pehle working session close na karein.

---

# 30. Root Login Security

Common recommended server setting:

```text
PermitRootLogin prohibit-password
```

Ye root password login disable karti hai, lekin mumkin hai ke root Key-Based Authentication allow ho.

Zyada strict option:

```text
PermitRootLogin no
```

Aam tor par administrators ko:

1. Normal user se login karna chahiye.
2. Administrative tasks ke liye `sudo` use karna chahiye.

---

# 31. Authorized Key Remove Karein

Server B par edit karein:

```bash
vim ~/.ssh/authorized_keys
```

Unwanted Public Key wali line delete karein.

Behtar hai key ko comment ya fingerprint se identify karein.

Removal ke baad matching Private Key us user ke taur par authenticate nahi kar sakegi.

---

# 32. Keys Mein Comments Add Karein

Key comment owner ya purpose identify karne mein madad karta hai.

Custom comment ke saath key generate karein:

```bash
ssh-keygen -t ed25519 -C "user1@serverA"
```

Automation ke liye:

```bash
ssh-keygen -t ed25519 -C "backup-service-serverA"
```

Comment khud security provide nahi karta, lekin key management aasaan banata hai.

---

# 33. Authorized Key Restrict Karein

`authorized_keys` entry mein restrictions add ki ja sakti hain.

Example:

```text
from="192.168.1.66",no-agent-forwarding,no-port-forwarding,no-pty ssh-ed25519 AAAAC3... user1@serverA
```

Possible restrictions:

| Restriction | Kaam |
|-------------|------|
| `from="IP"` | Key ko sirf specific source se allow kare |
| `no-agent-forwarding` | SSH agent forwarding disable kare |
| `no-port-forwarding` | Port forwarding disable kare |
| `no-pty` | Interactive terminal allocate na kare |
| `command="..."` | Specific command force kare |

Ye options automated service accounts ke liye bohot useful hain.

---

# 34. Common SSH Key Files

| File | Kaam |
|------|------|
| `~/.ssh/id_ed25519` | ED25519 Private Key |
| `~/.ssh/id_ed25519.pub` | ED25519 Public Key |
| `~/.ssh/id_rsa` | RSA Private Key |
| `~/.ssh/id_rsa.pub` | RSA Public Key |
| `~/.ssh/authorized_keys` | Allowed Public Keys store karti hai |
| `~/.ssh/config` | Per-user SSH client configuration |
| `~/.ssh/known_hosts` | Trusted SSH server Host Keys |

---

# 35. Host Keys Aur User Keys Mein Farq

In dono systems ko confuse na karein.

| Feature | Host Keys | User Keys |
|---------|-----------|-----------|
| Purpose | Server authenticate karna | User authenticate karna |
| Private Key Location | Server par `/etc/ssh/ssh_host_*` | Client par `~/.ssh/id_*` |
| Public Key Location | Server present karta hai aur client `known_hosts` mein save karta hai | Remote `authorized_keys` mein store hoti hai |
| Verification Direction | Server se client | User se server |

---

# 36. Complete SSH Security Flow

```text
Step 1: Client Server Ko Verify Karta Hai
        │
        └── Server Host Key known_hosts se compare hoti hai
                       │
                       ▼
Step 2: Server User Ko Verify Karta Hai
        │
        └── User Public Key authorized_keys mein check hoti hai
                       │
                       ▼
Step 3: Client Private-Key Possession Prove Karta Hai
                       │
                       ▼
Step 4: Encrypted SSH Session Open Hoti Hai
```

---

# 🧪 Practice Lab

## Step 1 – Server A Par User Verify Karein

```bash
whoami
```

Expected:

```text
user1
```

---

## Step 2 – Key Pair Generate Karein

```bash
ssh-keygen -t ed25519
```

Default location accept karein.

---

## Step 3 – Files Verify Karein

```bash
ls -la ~/.ssh
```

---

## Step 4 – Public Key Dekhein

```bash
cat ~/.ssh/id_ed25519.pub
```

---

## Step 5 – Public Key Copy Karein

```bash
ssh-copy-id user1@serverB
```

---

## Step 6 – Remote Permissions Verify Karein

Server B par:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## Step 7 – Key-Based Login Test Karein

Server A se:

```bash
ssh user1@serverB
```

---

## Step 8 – Identity Verify Karein

```bash
whoami
hostname
pwd
```

---

## Step 9 – Verbose Output Ke Saath Test Karein

```bash
ssh -v user1@serverB
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – SSH Abhi Bhi Remote Password Maang Raha Hai

Possible causes:

- Public Key ghalat user ke andar copy hui.
- File permissions incorrect hain.
- Ownership incorrect hai.
- `PubkeyAuthentication` disabled hai.
- Client wrong Private Key use kar raha hai.
- Public Key `authorized_keys` mein missing hai.
- SELinux context incorrect hai.

Check karein:

```bash
ssh -vv user1@serverB
```

---

### Scenario 2 – Remote Permissions Fix Karein

Server B par:

```bash
chmod 700 /home/user1/.ssh
chmod 600 /home/user1/.ssh/authorized_keys
chown -R user1:user1 /home/user1/.ssh
```

---

### Scenario 3 – SELinux Context Restore Karein

Rocky Linux ya RHEL par:

```bash
sudo restorecon -Rv /home/user1/.ssh
```

---

### Scenario 4 – Wrong Private Key Use Ho Rahi Hai

Key specify karein:

```bash
ssh -i ~/.ssh/id_ed25519 user1@serverB
```

Verbose mode:

```bash
ssh -vv -i ~/.ssh/id_ed25519 user1@serverB
```

---

### Scenario 5 – Public-Key Authentication Disabled Hai

Check karein:

```bash
sudo sshd -T | grep -i pubkeyauthentication
```

Zaroorat ho to configure karein:

```text
PubkeyAuthentication yes
```

Phir validate karein:

```bash
sudo sshd -t
```

Restart:

```bash
sudo systemctl restart sshd
```

---

### Scenario 6 – `ssh-copy-id` Available Nahi Hai

OpenSSH client tools install karein:

```bash
sudo dnf install -y openssh-clients
```

Ya manual copy command:

```bash
cat ~/.ssh/id_ed25519.pub | ssh user1@serverB 'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

---

### Scenario 7 – Private Key Compromise Ho Gayi

Har affected server par:

1. Edit karein:

   ```bash
   vim ~/.ssh/authorized_keys
   ```

2. Compromised Public Key remove karein.

3. Naya pair generate karein:

   ```bash
   ssh-keygen -t ed25519
   ```

4. Nayi Public Key copy karein.

5. SSH logs review karein:

   ```bash
   sudo journalctl -u sshd
   ```

---

# ⚠️ Security Best Practices

- ED25519 keys use karein jab supported hon.
- Private Keys ko passphrase se protect karein.
- Private Key kabhi share na karein.
- Important environments ke liye separate key use karein.
- Automation ke liye dedicated service accounts use karein.
- `authorized_keys` mein automation keys restrict karein.
- Unused Public Keys remove karein.
- `authorized_keys` regular review karein.
- OpenSSH updated rakhein.
- Password authentication tabhi disable karein jab Key-Based Authentication test ho chuki ho.
- `ssh-agent` carefully use karein.
- Ek administrator ki Private Key multiple users ya devices par copy na karein.

---

# 📌 Quick Revision

| Item | Kaam |
|------|------|
| Private Key | Client par secret rehti hai |
| Public Key | Remote server par copy hoti hai |
| `ssh-keygen` | Key pair generate karta hai |
| `ssh-copy-id` | Public Key remotely install karta hai |
| `authorized_keys` | Allowed Public Keys store karti hai |
| `id_ed25519` | ED25519 Private Key |
| `id_ed25519.pub` | ED25519 Public Key |
| `ssh -i KEY` | Specific Private Key use karta hai |
| `ssh-agent` | Unlocked keys temporarily store karta hai |
| `PubkeyAuthentication` | Server-side Public-Key authentication control karta hai |

---

# Common Commands

| Command | Kaam |
|---------|------|
| `ssh-keygen -t ed25519` | ED25519 key pair generate kare |
| `ssh-copy-id user@server` | Public Key copy kare |
| `cat ~/.ssh/id_ed25519.pub` | Public Key dekhe |
| `ssh user@server` | Key-Based Authentication test kare |
| `ssh -i KEY user@server` | Specific Private Key use kare |
| `ssh -v user@server` | SSH authentication debug kare |
| `ssh-add KEY` | Key ko `ssh-agent` mein add kare |
| `ssh-add -l` | Loaded agent keys list kare |
| `sshd -T` | Effective SSH server configuration dikhaye |
| `restorecon -Rv ~/.ssh` | SELinux contexts restore kare |

---

# 📖 Key Takeaways

- SSH Key-Based Authentication Public aur Private Key pair use karti hai.
- Private Key client par rehti hai aur secret honi chahiye.
- Public Key remote user ki `authorized_keys` file mein copy hoti hai.
- SSH verify karta hai ke client matching Private Key possess karta hai.
- Private Key server ko kabhi send nahi hoti.
- Same Public Key multiple servers par authorized ho sakti hai.
- Private Key compromise hone se har trusting server expose ho sakta hai.
- Keys safely install karne ke liye `ssh-copy-id` use karein.
- Correct ownership, permissions aur SELinux contexts bohot zaroori hain.
- Password login disable karne se pehle Key-Based Authentication test karein.

---

# 💡 Yaad Rakhein

> **SSH Key-Based Authentication Ko Lock Aur Proof System Ki Tarah Samjhein.**
>
> - **Public Key** server par install hoti hai.
> - **Private Key** user ke paas rehti hai.
> - Server check karta hai ke user matching Private Key ki possession prove kar sakta hai ya nahi.
> - Private Key network par kabhi send nahi hoti.
>
> **Golden Rule:**
>
> ```text
> Private Key = Secret Rakho
>
> Public Key = authorized_keys Mein Copy Karo
> ```
>
> **Jis ke paas Private Key aa jaye, woh un tamam servers tak access hasil kar sakta hai jo us ki matching Public Key trust karte hain.**