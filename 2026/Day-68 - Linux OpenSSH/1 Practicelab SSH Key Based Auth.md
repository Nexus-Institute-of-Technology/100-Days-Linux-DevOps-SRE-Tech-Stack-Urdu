# MODULE 08 – SSH Practice Lab 1
> **Do Linux Servers Ke Darmiyan SSH Key-Based Authentication Configure Karna (Roman Urdu)**

> **Scenario:** Server 1 se Server 2 tak `oradb` user ke liye passwordless SSH access configure karna.

---

# 📊 SSH Key-Based Authentication Diagram

![SSH Key-Based Authentication](/mnt/data/image(526).png)

Diagram ye dikhata hai:

```text
Server 1
oradb
   │
   │ SSH Key-Based Authentication
   ▼
Server 2
oradb
```

Maqsad ye hai ke Server 1 ka `oradb` user, remote account ka password enter kiye baghair Server 2 se connect kar sake.

---

# 🎯 Lab Ke Maqasid

Is practice lab mein aap seekhenge:

- Do Linux servers par same user create karna.
- Normal password-based SSH access test karna.
- SSH Public aur Private Key pair generate karna.
- Private Key aur Public Key ke darmiyan farq samajhna.
- Public Key ko remote server par copy karna.
- Passwordless SSH authentication configure karna.
- Remote `authorized_keys` file verify karna.
- Private Key ko passphrase ke saath protect karna.
- Passwordless login aur Private-Key passphrase ke darmiyan farq samajhna.
- SSH Key-Based Authentication ke common issues troubleshoot karna.

---

# 📖 Lab Scenario

Suppose aap Linux System Administrator hain.

Database team ne ye request submit ki hai:

> Server 1 par mojood `oradb` user ko Server 2 par SSH ke zariye login karte waqt `oradb` account ka password enter nahi karna chahiye.

Environment mein ye systems hain:

| System | Role | User |
|--------|------|------|
| Server 1 | SSH Client | `oradb` |
| Server 2 | SSH Server | `oradb` |

Same user dono systems par mojood hona chahiye.

---

# Access Ki Important Direction

Required SSH access:

```text
Server 1 ─────────► Server 2
```

Key pair generate hoga:

```text
Server 1
```

Public Key copy hogi:

```text
Server 2
```

---

# 1. Lab Requirements

Start karne se pehle confirm karein:

- Dono servers powered on hain.
- Dono servers network par communicate kar sakte hain.
- Server 2 par SSH service running hai.
- Firewall mein TCP port `22` allowed hai.
- Aap ke paas `root` ya `sudo` access hai.
- Server 2 ka correct hostname ya IP address maloom hai.

---

# 2. Example Lab Environment

In values ko apne actual systems ke mutabiq replace karein.

| Item | Server 1 | Server 2 |
|------|----------|----------|
| Hostname | `server1` | `server2` |
| IP Address | `192.168.1.66` | `192.168.1.67` |
| User | `oradb` | `oradb` |

---

# 3. Servers Verify Karein

Server 1 par:

```bash
hostname
```

```bash
hostname -I
```

Server 2 par:

```bash
hostname
```

```bash
hostname -I
```

Example:

```text
Server 1: 192.168.1.66
Server 2: 192.168.1.67
```

---

# 4. Network Connectivity Test Karein

Server 1 se Server 2 ko test karein:

```bash
ping -c 4 192.168.1.67
```

Expected result:

```text
4 packets transmitted, 4 received
```

Agar server response nahi deta to verify karein:

- IP address
- Network configuration
- Routing
- Firewall
- VM ki power state

---

# 5. Server 2 Par SSH Service Check Karein

Server 2 par:

```bash
systemctl status sshd
```

Agar service running nahi hai:

```bash
sudo systemctl enable --now sshd
```

Verify karein:

```bash
systemctl is-active sshd
```

Expected Output:

```text
active
```

---

# 6. Server 2 Par Firewall Check Karein

Run karein:

```bash
sudo firewall-cmd --list-services
```

Output mein ye hona chahiye:

```text
ssh
```

Agar SSH allowed nahi hai:

```bash
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

Verify karein:

```bash
sudo firewall-cmd --list-services
```

---

# 7. Server 1 Par `oradb` User Create Karein

Server 1 par `root` se login karein ya `sudo` use karein.

User create karein:

```bash
sudo useradd oradb
```

Password set karein:

```bash
sudo passwd oradb
```

Verify karein:

```bash
id oradb
```

Example Output:

```text
uid=1001(oradb) gid=1001(oradb) groups=1001(oradb)
```

---

# 8. Server 2 Par `oradb` User Create Karein

Server 2 par:

```bash
sudo useradd oradb
```

Password set karein:

```bash
sudo passwd oradb
```

Verify karein:

```bash
id oradb
```

---

# User Ke Bare Mein Important Note

Server 1 ka `oradb` account aur Server 2 ka `oradb` account do separate local accounts hain.

Har server ka apna hota hai:

- Password
- UID
- Group
- Home directory
- SSH configuration
- Authorized Keys

Username same ho sakta hai, lekin accounts independently manage hote hain.

---

# 9. Server 1 Par `oradb` Mein Switch Karein

Run karein:

```bash
su - oradb
```

User verify karein:

```bash
whoami
```

Expected Output:

```text
oradb
```

Current directory check karein:

```bash
pwd
```

Expected Output:

```text
/home/oradb
```

---

# 10. Password-Based SSH Login Test Karein

Server 1 par `oradb` user se Server 2 par connect karein:

```bash
ssh oradb@192.168.1.67
```

Pehli connection par ye message aa sakta hai:

```text
The authenticity of host '192.168.1.67' cannot be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Host-Key fingerprint verify karne ke baad type karein:

```text
yes
```

Phir remote account ka password maanga jayega:

```text
oradb@192.168.1.67's password:
```

Server 2 par configured `oradb` password enter karein.

---

# 11. Remote Login Verify Karein

Connect hone ke baad:

```bash
whoami
```

Expected:

```text
oradb
```

Run karein:

```bash
hostname
```

Expected:

```text
server2
```

Run karein:

```bash
pwd
```

Expected:

```text
/home/oradb
```

Server 1 par wapas aane ke liye:

```bash
exit
```

---

# Current Situation

Is stage par SSH work kar raha hai, lekin remote password abhi bhi maangta hai.

```text
Server 1
oradb
   │
   │ ssh oradb@server2
   ▼
Server 2
oradb
   │
   └── Remote password required
```

Agla step SSH Key-Based Authentication configure karna hai.

---

# 12. Server 1 Par SSH Key Pair Generate Karein

Confirm karein ke aap logged in hain:

```text
oradb
```

Verify:

```bash
whoami
```

Modern ED25519 key pair generate karein:

```bash
ssh-keygen -t ed25519
```

---

# Key Generation Prompts

Aap ko ye nazar aa sakta hai:

```text
Generating public/private ed25519 key pair.
```

Phir:

```text
Enter file in which to save the key (/home/oradb/.ssh/id_ed25519):
```

Default location accept karne ke liye press karein:

```text
Enter
```

Phir:

```text
Enter passphrase (empty for no passphrase):
```

Lab ke pehle hissa mein passphrase empty chhorne ke liye press karein:

```text
Enter
```

Confirm karne ke liye dobara:

```text
Enter
```

---

# 13. `ssh-keygen` Se Create Hone Wali Files

Run karein:

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

| File | Maqsad |
|------|--------|
| `id_ed25519` | Private Key |
| `id_ed25519.pub` | Public Key |

---

# 14. Private Key

Private Key hai:

```text
~/.ssh/id_ed25519
```

Isay:

- Server 1 par rehna chahiye.
- `oradb` ki ownership mein rehna chahiye.
- Server 2 par kabhi copy nahi karna chahiye.
- Kisi doosre person ke saath share nahi karna chahiye.
- Restrictive permissions ke saath protect karna chahiye.

Check karein:

```bash
ls -l ~/.ssh/id_ed25519
```

Typical permissions:

```text
-rw-------
```

Zaroorat ho to set karein:

```bash
chmod 600 ~/.ssh/id_ed25519
```

---

# ⚠️ Private Key Security

Jis shakhs ke paas Private Key aa jaye, woh un tamam servers par connect kar sakta hai jahan matching Public Key trusted hai.

Private Key ko kabhi:

- Email na karein.
- Ticket mein copy na karein.
- GitHub par upload na karein.
- Shared folder mein store na karein.
- Chat ke zariye send na karein.
- Destination server par copy na karein.

---

# 15. Public Key

Public Key hai:

```text
~/.ssh/id_ed25519.pub
```

Isay dekhne ke liye:

```bash
cat ~/.ssh/id_ed25519.pub
```

Example:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... oradb@server1
```

Public Key ko safely Server 2 par copy kiya ja sakta hai.

---

# 16. Public Key Server 2 Par Copy Karein

Use karein:

```bash
ssh-copy-id oradb@192.168.1.67
```

Server 2 account ka password ek aakhri dafa maanga ja sakta hai:

```text
oradb@192.168.1.67's password:
```

Successful installation ke baad message aa sakta hai:

```text
Number of key(s) added: 1
```

---

# Exact Public Key Specify Karein

Agar multiple key pairs hain to:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub oradb@192.168.1.67
```

---

# `ssh-copy-id` Kya Karta Hai?

Ye command:

```bash
ssh-copy-id oradb@192.168.1.67
```

Server 2 par aam tor par:

1. Remote `.ssh` directory create karta hai agar required ho.
2. `authorized_keys` file create karta hai agar required ho.
3. Public Key ko `authorized_keys` mein copy karta hai.
4. Suitable permissions apply karta hai.
5. Existing authorized keys ko preserve karta hai.

---

# 17. Passwordless SSH Login Test Karein

Server 1 se:

```bash
ssh oradb@192.168.1.67
```

Agar configuration correct hai:

- Remote `oradb` account password nahi maanga jayega.
- SSH session directly open ho jayegi.
- Aap Server 2 par `oradb` ke taur par login ho jayenge.

Verify karein:

```bash
whoami
hostname
pwd
```

Expected:

```text
oradb
server2
/home/oradb
```

---

# Passwordless Login Flow

```text
Server 1
oradb
Private Key
   │
   │ ssh oradb@server2
   ▼
Server 2
oradb
authorized_keys
   │
   └── Public Key Match Hoti Hai
            │
            ▼
       Login Allow Hota Hai
```

---

# 18. Server 2 Par Public Key Verify Karein

Server 2 par `oradb` ke taur par:

```bash
ls -la ~/.ssh
```

Aap ko ye file nazar aani chahiye:

```text
authorized_keys
```

Contents display karein:

```bash
cat ~/.ssh/authorized_keys
```

Ye line Server 1 ki Public Key se match karni chahiye:

```bash
cat ~/.ssh/id_ed25519.pub
```

Doosri command Server 1 par run karein.

---

# Important Clarification

Server 2 par Public Key alag file ke taur par is naam se copy nahi hoti:

```text
id_ed25519.pub
```

Is ke bajaye Public Key ka content append hota hai:

```text
~/.ssh/authorized_keys
```

---

# 19. Remote Permissions Verify Karein

Server 2 par:

```bash
ls -ld ~/.ssh
```

```bash
ls -l ~/.ssh/authorized_keys
```

Recommended permissions:

```text
~/.ssh                 700
~/.ssh/authorized_keys 600
```

Zaroorat ho to apply karein:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Ownership verify karein:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

Owner hona chahiye:

```text
oradb:oradb
```

Agar required ho to fix karein:

```bash
sudo chown -R oradb:oradb /home/oradb/.ssh
```

---

# 20. SELinux Context Restore Karein

Rocky Linux ya RHEL par correct SELinux labels restore karein:

```bash
sudo restorecon -Rv /home/oradb/.ssh
```

Ye tab useful hai jab directory ya files manually create ya copy hui hon.

---

# 21. Authentication Background Mein Kaise Kaam Karti Hai?

Simplified SSH Key-Based Authentication process:

1. Server 1 ka `oradb`, SSH connection start karta hai.
2. Server 2 apni SSH Host Key present karta hai.
3. Server 1, `known_hosts` ke zariye Server 2 ki identity verify karta hai.
4. Server 1 `oradb` ki Public Key identity offer karta hai.
5. Server 2 `/home/oradb/.ssh/authorized_keys` check karta hai.
6. Server 2 confirm karta hai ke Public Key authorized hai.
7. Server 1 matching Private Key se cryptographic signature create karke possession prove karta hai.
8. Server 2 Public Key se signature verify karta hai.
9. Login allow ho jata hai.

---

# Authentication Flow

```text
Client Public Key Offer Karta Hai
        │
        ▼
Server authorized_keys Check Karta Hai
        │
        ├── Key Missing
        │      └── Authentication Fail
        │
        └── Key Mil Jati Hai
               │
               ▼
Client Private Key Se
Authentication Data Sign Karta Hai
               │
               ▼
Server Public Key Se
Signature Verify Karta Hai
               │
               ▼
Authentication Successful
```

---

# Important Security Fact

Private Key:

```text
~/.ssh/id_ed25519
```

Server 2 ko kabhi transmit nahi hoti.

Ye Server 1 par hi rehti hai.

Sirf cryptographic proof send hota hai.

---

# 22. Protected Private Key Ke Saath Doosra User Create Karein

Ab ek aur user create karein:

```text
oracle
```

Is hissa mein passphrase-protected Private Key demonstrate ki jayegi.

---

# 23. Server 1 Par `oracle` Create Karein

Server 1 par:

```bash
sudo useradd oracle
```

Password set karein:

```bash
sudo passwd oracle
```

Verify karein:

```bash
id oracle
```

---

# 24. Server 2 Par `oracle` Create Karein

Server 2 par:

```bash
sudo useradd oracle
```

Password set karein:

```bash
sudo passwd oracle
```

Verify karein:

```bash
id oracle
```

---

# 25. Server 1 Par `oracle` Mein Switch Karein

Run karein:

```bash
su - oracle
```

Verify:

```bash
whoami
```

Expected:

```text
oracle
```

---

# 26. Passphrase-Protected Key Generate Karein

Run karein:

```bash
ssh-keygen -t ed25519
```

Default file location accept karein.

Jab prompt aaye:

```text
Enter passphrase (empty for no passphrase):
```

ek strong passphrase enter karein.

Production mein simple classroom passwords use na karein.

Same passphrase dobara enter karke confirm karein.

---

# Passphrase Kis Cheez Ko Protect Karti Hai?

Passphrase local Private Key file ko protect karti hai:

```text
~/.ssh/id_ed25519
```

Ye remote account password ko change nahi karti.

Ye Server 2 par copy nahi hoti.

---

# 27. Oracle Ki Public Key Copy Karein

Server 1 par `oracle` ke taur par:

```bash
ssh-copy-id oracle@192.168.1.67
```

Remote `oracle` account ka password ek aakhri dafa enter karein.

---

# 28. Oracle Ka SSH Login Test Karein

Run karein:

```bash
ssh oracle@192.168.1.67
```

Aap ko ye prompt mil sakta hai:

```text
Enter passphrase for key '/home/oracle/.ssh/id_ed25519':
```

Private-Key passphrase enter karein.

Remote account password nahi maanga jana chahiye.

---

# Passphrase Aur Remote Password Mein Farq

| Credential | Kya Protect Karta Hai |
|------------|-----------------------|
| Remote account password | Server 2 ka `oracle` account |
| Private-Key passphrase | Server 1 ki Private Key |

---

# Important Clarification

Passphrase-protected SSH key phir bhi Key-Based Authentication hi hai.

Ye password authentication ke barabar nahi hai.

Aap local Private Key ko unlock kar rahe hain, remote account password se authenticate nahi kar rahe.

---

# 29. Private-Key Passphrase Kyun Use Karein?

Suppose attacker ye file chura leta hai:

```text
/home/oracle/.ssh/id_ed25519
```

Agar key par passphrase nahi hai to attacker foran use karne ki koshish kar sakta hai.

Agar key strong passphrase se protected hai to attacker ko passphrase bhi chahiye hogi.

Ye additional security layer provide karti hai.

---

# 30. Repeated Passphrase Prompts Se Bachne Ke Liye `ssh-agent`

SSH agent start karein:

```bash
eval "$(ssh-agent -s)"
```

Key add karein:

```bash
ssh-add ~/.ssh/id_ed25519
```

Passphrase ek dafa enter karein.

Loaded keys verify karein:

```bash
ssh-add -l
```

Ab connect karein:

```bash
ssh oracle@192.168.1.67
```

Current agent session mein SSH dobara passphrase na maange.

---

# 31. Dono Lab Users Ka Comparison

| User | Private-Key Passphrase | Setup Ke Baad Remote Password |
|------|-------------------------|-------------------------------|
| `oradb` | Nahi | Nahi |
| `oracle` | Haan | Nahi |
| `oracle` with `ssh-agent` | Haan, agent mein enter hoti hai | Repeated prompt nahi |

---

# 32. Verify Karein SSH Kaun Si Key Use Kar Raha Hai

Run karein:

```bash
ssh -v oradb@192.168.1.67
```

In messages ko dekhein:

```text
Offering public key
Server accepts key
Authenticated using "publickey"
```

Zyada detail ke liye:

```bash
ssh -vv oradb@192.168.1.67
```

---

# 33. Specific Private Key Use Karein

Agar multiple keys hain:

```bash
ssh -i ~/.ssh/id_ed25519 oradb@192.168.1.67
```

Custom key ke liye:

```bash
ssh -i ~/.ssh/oradb_server2 oradb@192.168.1.67
```

---

# 34. SSH Client Alias Configure Karein

Server 1 par `oradb` user se edit karein:

```bash
vim ~/.ssh/config
```

Add karein:

```text
Host database-server
    HostName 192.168.1.67
    User oradb
    IdentityFile ~/.ssh/id_ed25519
```

File secure karein:

```bash
chmod 600 ~/.ssh/config
```

Connect karein:

```bash
ssh database-server
```

---

# 35. Server-Side Public-Key Authentication Check Karein

Server 2 par:

```bash
sudo sshd -T | grep -i pubkeyauthentication
```

Expected Output:

```text
pubkeyauthentication yes
```

Authorized Keys location check karein:

```bash
sudo sshd -T | grep -i authorizedkeysfile
```

Typical Output:

```text
authorizedkeysfile .ssh/authorized_keys .ssh/authorized_keys2
```

---

# 36. SSH Server Configuration Validate Karein

`sshd` restart karne se pehle:

```bash
sudo sshd -t
```

Agar output nahi aata to syntax aam tor par valid hai.

Sirf server configuration change hone par restart karein:

```bash
sudo systemctl restart sshd
```

---

# 37. Optional: Password Authentication Disable Karein

Sirf Key-Based Authentication successful test hone ke baad:

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

# ⚠️ Khud Ko Lock Out Hone Se Bachayein

Passwords disable karne se pehle:

1. Current SSH session open rakhein.
2. Doosra terminal open karein.
3. Key-Based Authentication test karein.
4. Administrative access confirm karein.
5. Correct key offered ho rahi hai, verify karein.
6. Firewall access confirm karein.
7. Naya login successful hone tak working session close na karein.

---

# 38. Complete Lab Workflow

```text
Server 1 Aur Server 2 Par User Create Karein
                │
                ▼
Password-Based SSH Login Test Karein
                │
                ▼
Server 1 Par Key Pair Generate Karein
                │
                ├── Private Key Server 1 Par Rehti Hai
                │
                └── Public Key Server 2 Par Copy Hoti Hai
                                │
                                ▼
                  ~/.ssh/authorized_keys
                                │
                                ▼
                     SSH Login Dobara Test Karein
                                │
                                ▼
                 Remote Password Required Nahi
```

---

# 🧪 Student Practice Tasks

## Task 1 – `oradb` Create Karein

Dono servers par:

```bash
sudo useradd oradb
sudo passwd oradb
```

---

## Task 2 – User Verify Karein

```bash
id oradb
```

---

## Task 3 – Normal SSH Test Karein

Server 1 se:

```bash
su - oradb
ssh oradb@192.168.1.67
```

---

## Task 4 – Key Pair Generate Karein

```bash
ssh-keygen -t ed25519
```

---

## Task 5 – Key Files Verify Karein

```bash
ls -la ~/.ssh
```

---

## Task 6 – Public Key Copy Karein

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub oradb@192.168.1.67
```

---

## Task 7 – Key-Based Login Test Karein

```bash
ssh oradb@192.168.1.67
```

---

## Task 8 – Remote File Verify Karein

Server 2 par:

```bash
cat ~/.ssh/authorized_keys
```

---

## Task 9 – Permissions Verify Karein

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

---

## Task 10 – Verbose Authentication Test Karein

```bash
ssh -v oradb@192.168.1.67
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – SSH Abhi Bhi Remote Password Maang Raha Hai

Run karein:

```bash
ssh -vv oradb@192.168.1.67
```

Possible reasons:

- Ghalat Public Key copy hui.
- Ghalat remote user use hua.
- Permissions incorrect hain.
- Ownership incorrect hai.
- Public-Key authentication disabled hai.
- Ghalat Private Key offer ho rahi hai.
- SELinux context problem hai.

---

### Scenario 2 – Remote Permissions Fix Karein

Server 2 par:

```bash
chmod 700 /home/oradb/.ssh
chmod 600 /home/oradb/.ssh/authorized_keys
sudo chown -R oradb:oradb /home/oradb/.ssh
```

---

### Scenario 3 – SELinux Context Restore Karein

```bash
sudo restorecon -Rv /home/oradb/.ssh
```

---

### Scenario 4 – `ssh-copy-id` Missing Hai

Install karein:

```bash
sudo dnf install -y openssh-clients
```

Ya manually copy karein:

```bash
cat ~/.ssh/id_ed25519.pub | ssh oradb@192.168.1.67 'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

---

### Scenario 5 – Wrong Private Key Offer Ho Rahi Hai

Key specify karein:

```bash
ssh -i ~/.ssh/id_ed25519 oradb@192.168.1.67
```

Debug karein:

```bash
ssh -vv -i ~/.ssh/id_ed25519 oradb@192.168.1.67
```

---

### Scenario 6 – Permission Denied (Publickey)

Server 2 par check karein:

```bash
sudo journalctl -u sshd -n 50
```

Ya:

```bash
sudo tail -n 50 /var/log/secure
```

---

### Scenario 7 – Public Key Wrong Account Mein Copy Hui

Confirm karein:

```bash
ssh-copy-id oradb@192.168.1.67
```

Key yahan honi chahiye:

```text
/home/oradb/.ssh/authorized_keys
```

Yahan nahi:

```text
/root/.ssh/authorized_keys
```

ya kisi doosre user ki Home Directory mein.

---

### Scenario 8 – `authorized_keys` Mein Duplicate Keys

Check karein:

```bash
sort ~/.ssh/authorized_keys | uniq -d
```

Duplicate keys aam tor par authentication ko break nahi karti, lekin management confusing bana deti hain.

Unnecessary duplicates carefully remove karein.

---

### Scenario 9 – Private Key Permissions Bohot Open Hain

Error ho sakta hai:

```text
WARNING: UNPROTECTED PRIVATE KEY FILE!
```

Fix karein:

```bash
chmod 600 ~/.ssh/id_ed25519
```

---

### Scenario 10 – Passphrase Bhool Gaye

Private-Key passphrase aam tor par recover nahi ki ja sakti.

Aap ko:

1. Naya key pair generate karna hoga.
2. Nayi Public Key Server 2 par copy karni hogi.
3. Purani Public Key `authorized_keys` se remove karni hogi.

---

# ⚠️ Security Best Practices

- Jab supported ho ED25519 keys use karein.
- Interactive users ki keys ko passphrase se protect karein.
- Private Keys sirf trusted client systems par rakhein.
- Private Keys kabhi share na karein.
- Mukhtalif purposes ke liye separate keys use karein.
- Automation ke liye dedicated service accounts use karein.
- Automation keys ko `authorized_keys` mein restrict karein.
- Unused Public Keys remove karein.
- `authorized_keys` regularly review karein.
- Password authentication sirf successful testing ke baad disable karein.
- `ssh-agent` carefully use karein.
- Critical keys ko organization ki policy ke mutabiq securely back up karein.

---

# 📌 Quick Revision

| Item | Maqsad |
|------|--------|
| `ssh-keygen` | Public aur Private Keys generate kare |
| Private Key | Server 1 par rehti hai |
| Public Key | Server 2 par copy hoti hai |
| `ssh-copy-id` | Public Key remotely install kare |
| `authorized_keys` | Approved Public Keys store kare |
| Passphrase | Local Private Key ko protect kare |
| Remote password | Remote user account ko protect kare |
| `ssh-agent` | Unlocked key temporarily store kare |
| `ssh -v` | SSH authentication troubleshoot kare |
| `restorecon` | SELinux file contexts restore kare |

---

# Common Commands

| Command | Kaam |
|---------|------|
| `ssh-keygen -t ed25519` | ED25519 key pair generate kare |
| `ssh-copy-id user@server` | Public Key copy kare |
| `ssh-copy-id -i key.pub user@server` | Specific Public Key copy kare |
| `ssh user@server` | Key-Based Authentication test kare |
| `ssh -i key user@server` | Specific Private Key use kare |
| `ssh -v user@server` | Authentication details display kare |
| `cat ~/.ssh/authorized_keys` | Approved Public Keys dekhe |
| `chmod 700 ~/.ssh` | SSH directory secure kare |
| `chmod 600 ~/.ssh/authorized_keys` | Authorized Keys file secure kare |
| `restorecon -Rv ~/.ssh` | SELinux contexts restore kare |

---

# 📖 Key Takeaways

- `oradb` user dono servers par exist karna chahiye.
- Key pair source system, yani Server 1 par generate hota hai.
- Private Key Server 1 par rehti hai.
- Public Key Server 2 par copy hoti hai.
- `ssh-copy-id` Public Key ko `authorized_keys` mein place karta hai.
- Successful configuration ke baad remote account password required nahi hota.
- Key passphrase local Private Key ko protect karti hai.
- Private Key network par kabhi send nahi hoti.
- Correct permissions, ownership aur SELinux contexts bohot zaroori hain.
- Password authentication disable karne se pehle connection zaroor test karein.

---

# 💡 Yaad Rakhein

> **SSH Key-Based Authentication Ko Ek Secure Identity-Proof System Ki Tarah Samjhein.**
>
> - Server 1 **Private Key** rakhta hai.
> - Server 2 matching **Public Key** store karta hai.
> - Server 1 prove karta hai ke us ke paas Private Key hai.
> - Server 2 Public Key se us proof ko verify karta hai.
>
> **Golden Rule:**
>
> ```text
> Keys Source Server Par Generate Karein
>
> Private Key Source Server Par Rakhein
>
> Public Key Destination Server Ke authorized_keys Mein Copy Karein
> ```
>
> **Private Key ko destination server par kabhi copy na karein.**P