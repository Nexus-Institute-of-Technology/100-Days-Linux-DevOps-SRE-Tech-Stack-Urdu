# MODULE 08 – SSH Host Keys
> **SSH Server Authentication, Host-Key Verification Aur `known_hosts` Ko Samajhna (Roman Urdu)**

---

# 🎯 Learning Objectives

Is lesson mein aap seekhenge:

- SSH Host Keys kya hoti hain.
- SSH client, SSH server ki identity ko kaise verify karta hai.
- Host Keys aur User Authentication Keys ke darmiyan farq.
- Pehli SSH connection ke dauran kya hota hai.
- Server Host Keys kahan store hoti hain.
- Client trusted Host Keys kahan save karta hai.
- `known_hosts` file kis tarah kaam karti hai.
- SSH Host-Key warning kyun display karta hai.
- Host Keys Man-in-the-Middle attack se bachne mein kaise madad karti hain.
- Strict Host-Key Checking ko safely kaise configure karna hai.

---

# 📖 Introduction

SSH systems ke darmiyan secure aur encrypted communication provide karta hai.

Jab SSH client kisi SSH server se connect karta hai to do important security checks hote hain:

1. Client server ki identity verify karta hai.
2. Server user ki identity verify karta hai.

SSH **Host Keys** pehle kaam ke liye use hoti hain:

> **SSH server ko client ke saamne authenticate karna.**

Is se client confirm karta hai ke woh sahi server se connect ho raha hai, na ke kisi attacker se jo us server banne ka drama kar raha ho.

---

# Important Terminology

Sahi term hai:

```text
SSH Host Keys
```

Ye nahi:

```text
SSH Hot Keys
```

Host Key SSH server ki identity represent karti hai.

---

# 1. SSH Host Key Kya Hai?

SSH Host Key ek cryptographic key pair hoti hai jo SSH server ki hoti hai.

Is pair mein hota hai:

- Private Host Key
- Public Host Key

Private key SSH server par secret rehti hai.

Public key connection setup ke dauran SSH clients ko present ki jati hai.

---

# Host-Key Pair

```text
SSH Server
│
├── Private Host Key
│   └── Server par secret rehni chahiye
│
└── Public Host Key
    └── Connect karne wale clients ke saath share hoti hai
```

---

# 2. SSH Host Keys Ka Maqsad

SSH Host Keys use hoti hain:

- SSH server ko identify karne ke liye.
- Ye prove karne ke liye ke server ke paas us ki private key mojood hai.
- Server impersonation rokne ke liye.
- Unexpected server identity changes detect karne ke liye.
- Man-in-the-Middle attacks se protection ke liye.

Host Key ko SSH server ka identity card samjha ja sakta hai.

---

# 3. Host Keys Aur User SSH Keys Mein Farq

Host Keys aur User SSH Keys mukhtalif cheezen hain.

| Key Type | Maqsad |
|----------|--------|
| SSH Host Key | Server ko client ke saamne authenticate karti hai |
| User SSH Key | User ko server ke saamne authenticate karti hai |

---

# SSH Authentication Direction

```text
Host Key
Server ─────────────► Client
Server ki identity prove karti hai

User Key Ya Password
Client ─────────────► Server
User ki identity prove karta hai
```

---

# 4. SSH Encryption Kaise Establish Hoti Hai?

Server Host Key important hoti hai, lekin woh akeli poori SSH session ko directly encrypt nahi karti.

Simplified SSH connection kuch is tarah kaam karti hai:

1. Client SSH server se contact karta hai.
2. Client aur server encryption algorithms negotiate karte hain.
3. Dono cryptographic key exchange perform karte hain.
4. Temporary symmetric session keys create hoti hain.
5. Server apni Host Key ke zariye identity prove karta hai.
6. Client server ki Host Key verify karta hai.
7. User password ya SSH key ke zariye authenticate hota hai.
8. Encrypted session start ho jati hai.

---

# Simplified SSH Connection Flow

```text
SSH Client
    │
    │ TCP Port 22 Par Connect Karta Hai
    ▼
SSH Server
    │
    │ Host-Key Information Bhejta Hai
    ▼
Client Server Ki Identity Verify Karta Hai
    │
    │ Key Exchange Session Keys Banata Hai
    ▼
Encrypted Communication Channel
    │
    │ User Authentication
    ▼
Secure Remote Shell
```

---

# 5. Practice Lab Environment

Suppose lab mein ye systems hain:

| Server | Hostname | Example IP |
|--------|----------|------------|
| Server 1 | `server1` | `192.168.1.66` |
| Server 2 | `server2` | `192.168.1.67` |

Ek user:

```text
manish
```

Server 1 par logged in hai aur Server 2 se connect karna chahta hai.

---

# 6. SSH Server Se Connect Karein

Server 1 se run karein:

```bash
ssh manish@192.168.1.67
```

Agar local aur remote username same hai to aap ye bhi use kar sakte hain:

```bash
ssh 192.168.1.67
```

SSH default tor par current local username use karega.

---

# 7. Pehli SSH Connection

Jab aap pehli dafa kisi server se connect karte hain to SSH ye message dikha sakta hai:

```text
The authenticity of host '192.168.1.67 (192.168.1.67)' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Is message ka matlab hai:

- Client ne pehle kabhi is server ko trust nahi kiya.
- Server ki Host Key local system par saved nahi hai.
- SSH pooch raha hai ke kya aap is server identity par trust karte hain.

---

# 8. Host-Key Fingerprint Verify Karein

Type karne se pehle:

```text
yes
```

fingerprint ko trusted tareeqe se verify karna chahiye.

Misal ke taur par Server 2 par directly login karke run karein:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

Example Output:

```text
256 SHA256:xxxxxxxxxxxxxxxxxxxxxxxx root@server2 (ED25519)
```

Is fingerprint ko Server 1 par display hone wale fingerprint ke saath compare karein.

Agar dono match karte hain to server identity confirm hai.

---

# Fingerprint Verification Kyun Important Hai?

Agar aap unverified Host Key accept kar lein to mumkin hai ke aap trust kar rahe hon:

- Ghalat server ko.
- Reconfigured system ko.
- Same IP use karne wali doosri machine ko.
- Malicious Man-in-the-Middle system ko.

---

# 9. Host Key Accept Karein

Fingerprint confirm karne ke baad type karein:

```text
yes
```

Aap ko ye message nazar aa sakta hai:

```text
Warning: Permanently added '192.168.1.67' (ED25519) to the list of known hosts.
```

Host Key client ki is file mein store ho jati hai:

```text
~/.ssh/known_hosts
```

Is ke baad SSH remote user ke authentication credentials maangta hai.

Example:

```text
manish@192.168.1.67's password:
```

---

# 10. Client Ki `known_hosts` File

Client trusted server Host Keys is file mein store karta hai:

```text
~/.ssh/known_hosts
```

User `manish` ke liye ye path ho sakta hai:

```text
/home/manish/.ssh/known_hosts
```

File dekhne ke liye:

```bash
cat ~/.ssh/known_hosts
```

---

# `known_hosts` Ka Maqsad

`known_hosts` file SSH client ko yaad rakhne mein madad karti hai:

> **Kaunsi Host Key kis remote server ki hai.**

Future connections par SSH compare karta hai:

- Server ke zariye abhi present ki gayi Host Key.
- `known_hosts` mein pehle se saved Host Key.

---

# Known-Hosts Comparison

```text
Server Current Host Key Present Karta Hai
                │
                ▼
Client Saved Host Key Read Karta Hai
~/.ssh/known_hosts Se
                │
                ▼
Keys Compare Hoti Hain
        │
        ├── Match ─────► Connection Continue Hoti Hai
        │
        └── Match Nahi ─► Security Warning
```

---

# 11. Server Host-Key Files

SSH server aam tor par Host Keys is directory ke andar store karta hai:

```text
/etc/ssh/
```

Common Host-Key files:

```text
/etc/ssh/ssh_host_ed25519_key
/etc/ssh/ssh_host_ed25519_key.pub
/etc/ssh/ssh_host_ecdsa_key
/etc/ssh/ssh_host_ecdsa_key.pub
/etc/ssh/ssh_host_rsa_key
/etc/ssh/ssh_host_rsa_key.pub
```

---

# Private Aur Public Host-Key Files

| File Ending | Matlab |
|-------------|--------|
| `.pub` ke baghair | Private Host Key |
| `.pub` ke saath | Public Host Key |

Example:

```text
ssh_host_ed25519_key
```

private key hai.

```text
ssh_host_ed25519_key.pub
```

public key hai.

---

# ⚠️ Important Security Rule

Server ki Private Host Key kabhi share na karein.

Private Host-Key files server par protected rehni chahiye.

Permissions check karein:

```bash
ls -l /etc/ssh/ssh_host_*
```

Private keys aam tor par sirf `root` read kar sakta hai.

---

# 12. Server Ki Public Host Key Dekhein

Server 2 par:

```bash
sudo cat /etc/ssh/ssh_host_ed25519_key.pub
```

Example:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... root@server2
```

Lekin `known_hosts` ki raw line bilkul same nazar na aaye, kyun ke:

- Hostname ya IP include ho sakta hai.
- Hostnames hashed ho sakte hain.
- Storage format mukhtalif ho sakta hai.
- Multiple key algorithms mojood ho sakti hain.

Sab se safe comparison fingerprint ke zariye hai.

---

# 13. Server Host-Key Fingerprint Display Karein

Server 2 par:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

ECDSA Host Key ke liye:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ecdsa_key.pub
```

RSA Host Key ke liye:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_rsa_key.pub
```

---

# 14. Future SSH Connections

Agli dafa Manish connect kare:

```bash
ssh manish@192.168.1.67
```

to SSH client compare karega:

```text
known_hosts Mein Saved Host Key
             VS
Server 2 Ki Presented Host Key
```

Agar dono match karein:

- Client server identity trust karega.
- First-time confirmation nahi maangi jayegi.
- User authentication continue hogi.
- Encrypted SSH session establish ho jayegi.

---

# 15. Agar Host Key Change Ho Jaye To Kya Hota Hai?

Agar saved Host Key aur current server Host Key match na karein to SSH ye warning dikha sakta hai:

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
```

Aap ye bhi dekh sakte hain:

```text
Host key verification failed.
```

---

# Host Key Change Hone Ki Common Wajahen

Host Key change ho sakti hai agar:

- Server reinstall hua ho.
- Operating system rebuild hua ho.
- SSH Host Keys regenerate hui hon.
- VM different image se restore hui ho.
- IP address kisi doosre server ko mil gaya ho.
- DNS record kisi doosre system ki taraf point kar raha ho.
- Server replace hua ho.
- Man-in-the-Middle attack ho raha ho.

---

# ⚠️ Warning Ko Automatically Ignore Na Karein

Changed Host Key ek security warning hai.

Old key ko foran remove na karein.

Pehle verify karein:

- Kya ye sahi server hai?
- Kya server recently rebuild hua?
- Kya IP address reassign hua?
- Kya administrator ne Host Keys regenerate ki?
- Kya new fingerprint real server ke fingerprint se match karta hai?

---

# 16. Stored Host Key Check Karein

Local `known_hosts` file mein search karein:

```bash
ssh-keygen -F 192.168.1.67
```

Hostname se connect karte hon to:

```bash
ssh-keygen -F server2
```

---

# 17. Old Host Key Safely Remove Karein

Sirf tab jab aap confirm kar lein ke change legitimate hai:

```bash
ssh-keygen -R 192.168.1.67
```

Hostname ke liye:

```bash
ssh-keygen -R server2
```

Phir reconnect karein:

```bash
ssh manish@192.168.1.67
```

New fingerprint verify karne ke baad hi accept karein.

---

# 18. `known_hosts` Ko Manually Corrupt Na Karein

Technically aap ye file edit kar sakte hain:

```text
~/.ssh/known_hosts
```

lekin Host-Key changes test karne ka recommended tareeqa nahi hai.

Safe removal ke liye use karein:

```bash
ssh-keygen -R hostname_or_ip
```

Random characters change karne se realistic Host-Key mismatch ke bajaye invalid-file ya parsing errors aa sakte hain.

---

# 19. Strict Host-Key Checking

SSH clients mein ek setting hoti hai:

```text
StrictHostKeyChecking
```

Ye **client-side setting** hai.

Ye configure hoti hai:

```text
/etc/ssh/ssh_config
```

ya:

```text
~/.ssh/config
```

Ye is file mein nahi hoti:

```text
/etc/ssh/sshd_config
```

kyun ke `sshd_config` SSH server ko control karti hai, client ko nahi.

---

# 20. `StrictHostKeyChecking` Ki Values

| Value | Behavior |
|-------|----------|
| `yes` | New Host Keys automatically add nahi hoti; changed keys reject hoti hain |
| `ask` | New Host Key add karne se pehle poochta hai; changed keys reject hoti hain |
| `accept-new` | New hosts automatically accept hote hain, changed keys reject hoti hain |
| `no` | New keys automatically accept hoti hain aur kuch changed-key situations warnings ke saath allow ho sakti hain |

Default behavior aam tor par is jaisa hota hai:

```text
ask
```

---

# 21. Ek User Ke Liye Strict Checking Configure Karein

Edit karein:

```bash
vim ~/.ssh/config
```

Add karein:

```text
Host *
    StrictHostKeyChecking yes
```

File secure karein:

```bash
chmod 600 ~/.ssh/config
```

---

# `yes` Ke Saath Kya Hota Hai?

Agar setting ho:

```text
StrictHostKeyChecking yes
```

to SSH:

- Unknown Host Keys reject karega.
- Changed Host Keys reject karega.
- Interactive acceptance allow nahi karega.

Example error:

```text
No ED25519 host key is known for 192.168.1.67 and you have requested strict checking.
Host key verification failed.
```

---

# 22. Sirf Ek Server Ke Liye Strict Checking Configure Karein

Zyada practical configuration:

```text
Host server2
    HostName 192.168.1.67
    User manish
    StrictHostKeyChecking yes
```

Ab connect karein:

```bash
ssh server2
```

---

# 23. System-Wide Client Configuration

Tamam users ke liye edit karein:

```bash
sudo vim /etc/ssh/ssh_config
```

Ya is directory ke andar file create karein:

```text
/etc/ssh/ssh_config.d/
```

Example:

```bash
sudo vim /etc/ssh/ssh_config.d/99-host-key-checking.conf
```

Add karein:

```text
Host *
    StrictHostKeyChecking yes
```

> Client configuration change karne ke baad aam tor par `sshd` restart karna zaroori nahi hota.

---

# Important Correction

Client settings change karne ke baad ye required nahi:

```bash
systemctl restart sshd
```

`sshd` restart sirf SSH server configuration change karne par relevant hota hai, jaise:

```text
/etc/ssh/sshd_config
```

Client settings jaise `~/.ssh/config` ke liye sirf nayi SSH connection start karein.

---

# 24. Effective SSH Client Configuration Check Karein

Run karein:

```bash
ssh -G server2 | grep -i stricthostkeychecking
```

Example Output:

```text
stricthostkeychecking true
```

User Known Hosts file setting check karne ke liye:

```bash
ssh -G server2 | grep -i userknownhostsfile
```

---

# 25. Trusted Host Key Preload Karein

Automated environments mein Host Key preload karne ke liye use hota hai:

```bash
ssh-keyscan
```

Example:

```bash
ssh-keyscan -H 192.168.1.67 >> ~/.ssh/known_hosts
```

Lekin:

> `ssh-keyscan` Host Key retrieve karta hai, lekin independently ye prove nahi karta ke key trustworthy hai.

Fingerprint ko trusted channel se verify karna zaroori hai.

---

# Safer Preloading Workflow

```bash
ssh-keyscan -t ed25519 192.168.1.67 > /tmp/server2.key
```

Fingerprint display karein:

```bash
ssh-keygen -lf /tmp/server2.key
```

Server 2 ke fingerprint se compare karein:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

Agar dono match karein to add karein:

```bash
cat /tmp/server2.key >> ~/.ssh/known_hosts
```

---

# 26. `known_hosts` Mein Hostnames Hash Karein

OpenSSH hostnames ko hashed form mein store kar sakta hai.

Hashed entry kuch is tarah dikhti hai:

```text
|1|xxxxxxxx|yyyyyyyy ssh-ed25519 AAAAC3...
```

Is se agar koi file read kare to usay easily pata nahi chalega ke aap kin servers se connect karte hain.

Current file hash karne ke liye:

```bash
ssh-keygen -H -f ~/.ssh/known_hosts
```

---

# 27. Host-Key Verification Test Karein

## Step 1: Existing Lab Entry Remove Karein

Confirm karne ke baad ke ye sirf lab system hai:

```bash
ssh-keygen -R 192.168.1.67
```

---

## Step 2: Dobarah Connect Karein

```bash
ssh manish@192.168.1.67
```

Aap ko first-time Host-Key prompt milna chahiye.

---

## Step 3: Fingerprint Verify Karein

Server 2 par:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

---

## Step 4: Key Accept Karein

Type karein:

```text
yes
```

---

## Step 5: Local Entry Verify Karein

```bash
ssh-keygen -F 192.168.1.67
```

---

# 28. Interview Questions

## Question 1: SSH Host Key Kya Hai?

**Answer:**

SSH Host Key ek cryptographic key hai jo SSH server ko client ke saamne authenticate karti hai.

---

## Question 2: Server Host Keys Kahan Store Hoti Hain?

**Answer:**

```text
/etc/ssh/ssh_host_*
```

---

## Question 3: Client Trusted Host Keys Kahan Store Karta Hai?

**Answer:**

```text
~/.ssh/known_hosts
```

---

## Question 4: Pehli SSH Connection Par Kya Hota Hai?

**Answer:**

Server Host Key present karta hai, client fingerprint display karta hai aur user decide karta hai ke usay trust karke save karna hai ya nahi.

---

## Question 5: Future Connections Par Kya Hota Hai?

**Answer:**

Client server ki current Host Key ko `known_hosts` mein saved key ke saath compare karta hai.

---

## Question 6: SSH Changed Host Key Warning Kyun Deta Hai?

**Answer:**

Server rebuild, replace, reconfigure hua ho sakta hai, ya attacker server ki identity impersonate kar raha ho sakta hai.

---

## Question 7: `StrictHostKeyChecking` Kya Hai?

**Answer:**

Ye SSH client setting hai jo unknown aur changed Host Keys ke handling rules control karti hai.

---

## Question 8: Kya `StrictHostKeyChecking` `sshd_config` Mein Configure Hoti Hai?

**Answer:**

Nahi. Ye client setting hai jo configure hoti hai:

```text
~/.ssh/config
```

ya:

```text
/etc/ssh/ssh_config
```

---

# 29. Host-Key Verification Flow

```text
Client Server Se Connect Karta Hai
        │
        ▼
Server Host Key Present Karta Hai
        │
        ▼
Client known_hosts Check Karta Hai
        │
        ├── Existing Key Nahi Hai
        │       │
        │       └── User Se Poochta Hai Ya Client Policy Apply Karta Hai
        │
        ├── Matching Key
        │       │
        │       └── Connection Continue Karta Hai
        │
        └── Different Key
                │
                └── Security Warning Deta Hai Aur Reject Karta Hai
```

---

# 30. First Connection Aur Changed Key Mein Farq

| Situation | Matlab | Normal SSH Behavior |
|-----------|--------|---------------------|
| Unknown Host Key | Client pehli dafa connect kar raha hai | Confirmation maangta hai |
| Matching Host Key | Server identity saved key se match karti hai | Connection continue hoti hai |
| Changed Host Key | Server identity saved key se different hai | Warning deta hai aur reject karta hai |

---

# 🧪 Practice Exercises

## Exercise 1 – Server Host Keys Display Karein

Server 2 par:

```bash
ls -l /etc/ssh/ssh_host_*
```

---

## Exercise 2 – ED25519 Fingerprint Display Karein

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

---

## Exercise 3 – Server 1 Se Connect Karein

```bash
ssh manish@192.168.1.67
```

---

## Exercise 4 – Client Ki Known Hosts File Dekhein

```bash
cat ~/.ssh/known_hosts
```

---

## Exercise 5 – Stored Host Key Dhoondein

```bash
ssh-keygen -F 192.168.1.67
```

---

## Exercise 6 – Lab Host Entry Remove Karein

```bash
ssh-keygen -R 192.168.1.67
```

---

## Exercise 7 – Server 2 Ke Liye Strict Checking Enable Karein

Edit karein:

```bash
vim ~/.ssh/config
```

Add karein:

```text
Host server2
    HostName 192.168.1.67
    User manish
    StrictHostKeyChecking yes
```

---

## Exercise 8 – Configuration Test Karein

```bash
ssh server2
```

---

## Exercise 9 – Effective Setting Check Karein

```bash
ssh -G server2 | grep -i stricthostkeychecking
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – First-Time Connection Prompt

Message:

```text
The authenticity of host cannot be established.
```

Action:

1. Real server fingerprint hasil karein.
2. Displayed fingerprint se compare karein.
3. Sirf match hone par accept karein.

---

### Scenario 2 – Remote Host Identification Changed

Message:

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

Action:

1. Stop karein aur investigate karein.
2. Confirm karein ke server rebuild ya replace hua hai ya nahi.
3. New fingerprint verify karein.
4. Confirmation ke baad hi old key remove karein.

```bash
ssh-keygen -R hostname_or_ip
```

---

### Scenario 3 – Strict Checking Unknown Server Ko Reject Kar Raha Hai

Message:

```text
No ED25519 host key is known and you have requested strict checking.
```

Action:

- Real Host Key hasil karein.
- Fingerprint verify karein.
- Verified key ko `known_hosts` mein add karein.
- Phir reconnect karein.

---

### Scenario 4 – `known_hosts` File Permissions Ghalat Hain

SSH directory aur file permissions fix karein:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/known_hosts
```

---

### Scenario 5 – Server Host-Key Files Missing Hain

Check karein:

```bash
ls -l /etc/ssh/ssh_host_*
```

Missing keys generate karein:

```bash
sudo ssh-keygen -A
```

Phir SSH server restart karein:

```bash
sudo systemctl restart sshd
```

> New Host Keys generate karne se existing clients ko changed Host-Key warning milegi.

---

### Scenario 6 – IP Aur Hostname Se Connect Karne Par Separate Entries

SSH separate entries store kar sakta hai:

```text
192.168.1.67
```

aur:

```text
server2
```

Dono search karein:

```bash
ssh-keygen -F 192.168.1.67
ssh-keygen -F server2
```

---

# ⚠️ Security Best Practices

- First connection par Host-Key fingerprints verify karein.
- Bina verify kiye blindly `yes` na type karein.
- Har changed Host-Key warning investigate karein.
- Server ki Private Host Keys protect karein.
- Private Host Keys clients ko kabhi copy na karein.
- High-security environments mein `StrictHostKeyChecking yes` use karein.
- Automation mein `accept-new` carefully use karein.
- Sensitive systems ke liye `StrictHostKeyChecking no` se bachein.
- Trusted Host Keys distribute karne ke liye configuration management use karein.
- `known_hosts` ko unauthorized modification se protect karein.

---

# 📌 Quick Revision

| Item | Maqsad |
|------|--------|
| SSH Host Key | SSH server ki identity confirm karti hai |
| Server Private Key | Server par secret rehti hai |
| Server Public Key | Clients ko present ki jati hai |
| `~/.ssh/known_hosts` | Trusted server Host Keys store karti hai |
| `/etc/ssh/ssh_host_*` | Server Host-Key files store karta hai |
| `ssh-keygen -lf FILE` | Key fingerprint display karta hai |
| `ssh-keygen -F HOST` | Saved Host Key search karta hai |
| `ssh-keygen -R HOST` | Saved Host Key remove karta hai |
| `StrictHostKeyChecking` | Client Host-Key verification control karta hai |
| Host-Key Mismatch | Rebuild, replacement ya attack ho sakta hai |

---

# Common Commands

| Command | Kaam |
|---------|------|
| `ssh user@server` | SSH server se connect kare |
| `cat ~/.ssh/known_hosts` | Trusted Host Keys dekhe |
| `ssh-keygen -F server` | `known_hosts` mein search kare |
| `ssh-keygen -R server` | Old Host-Key entry remove kare |
| `ssh-keygen -lf key.pub` | Key fingerprint display kare |
| `ls -l /etc/ssh/ssh_host_*` | Server Host Keys list kare |
| `ssh -G server` | Effective client settings display kare |
| `ssh-keyscan server` | Server Host Key retrieve kare |
| `sshd -t` | SSH server configuration validate kare |
| `ssh-keygen -A` | Missing server Host Keys generate kare |

---

# 📖 Key Takeaways

- SSH Host Keys server ko client ke saamne authenticate karti hain.
- Host Keys, User Authentication Keys se mukhtalif hoti hain.
- Client trusted Host Keys ko `~/.ssh/known_hosts` mein store karta hai.
- Server apni Host Keys `/etc/ssh/` ke andar store karta hai.
- Har connection par client saved key ko current server key se compare karta hai.
- Changed Host Key legitimate rebuild ya security attack dono indicate kar sakti hai.
- New ya changed key trust karne se pehle fingerprint verify karein.
- `StrictHostKeyChecking` client-side setting hai.
- Client configuration change karne ke baad `sshd` restart karna zaroori nahi.
- `known_hosts` ko randomly edit ya corrupt karne ke bajaye `ssh-keygen -R` use karein.

---

# 💡 Yaad Rakhein

> **SSH Host Key Ko SSH Server Ka Identity Card Samjhein.**
>
> - Server apna identity card present karta hai.
> - Client check karta hai ke kya us ne ye identity pehle dekhi hai.
> - Agar identity match kare to connection continue hoti hai.
> - Agar identity unexpectedly change ho to SSH warning deta hai.
>
> **Golden Host-Key Flow:**
>
> ```text
> Server Host Key Present Karta Hai
>           │
>           ▼
> Client known_hosts Check Karta Hai
>           │
>           ├── Match ─────► Trust Aur Continue
>           │
>           ├── New ───────► Accept Karne Se Pehle Verify
>           │
>           └── Changed ───► Stop Aur Investigate
> ```
>
> **SSH Host-Key warning ko kabhi ignore na karein jab tak aap verify na kar lein ke server ki identity kyun change hui.**