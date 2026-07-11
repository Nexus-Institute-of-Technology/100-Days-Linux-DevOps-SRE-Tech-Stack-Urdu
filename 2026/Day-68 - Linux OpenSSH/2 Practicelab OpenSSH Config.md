# MODULE 08 – Practice Lab OpenSSH Server Configuration
> **Linux Par OpenSSH Server Ko Configure Aur Secure Karna**

---

# 🎯 Learning Objectives

Is module mein aap seekhenge:

- OpenSSH Server service kya hoti hai.
- SSH service ko kaunsa daemon provide karta hai.
- SSH Server configuration file kahan mojood hoti hai.
- Default OpenSSH configuration ko harden karne ki zarurat kyun padti hai.
- Direct root login ko normally restrict kyun karna chahiye.
- Password authentication ko disable kyun kiya jata hai.
- SSH Key-Based Authentication security ko kaise improve karti hai.
- Normal user login ko mandatory bana kar accountability kaise improve hoti hai.
- `PermitRootLogin` setting kis tarah kaam karti hai.
- SSH service restart karne se pehle configuration ko validate kaise kiya jata hai.

---

# 📖 Introduction

OpenSSH Linux systems ko **secure aur encrypted remote access** provide karta hai.

SSH Server ka component is daemon ke zariye provide hota hai:

```text
sshd
```

`sshd` service incoming SSH connections ko listen karti hai, users ko authenticate karti hai aur secure remote sessions create karti hai.

Default OpenSSH Server configuration bohot se environments ke liye theek kaam karti hai. Lekin production environments mein administrators security ko mazboot banane ke liye is configuration mein changes karte hain.

Common hardening changes ye hain:

- Direct root login ko restrict karna.
- Password authentication ko disable karna.
- SSH Key-Based Authentication ko mandatory banana.
- Sirf specific users ya groups ko allow karna.
- Login timeout aur authentication settings ko customize karna.
- SSH logs aur failed login attempts ko monitor karna.

---

# 1. SSH Daemon Kya Hai?

SSH Server daemon ka naam hai:

```text
sshd
```

Ye OpenSSH Server service provide karta hai.

Ye daemon in cheezon ka zimmedar hota hai:

- Incoming SSH connections ko listen karna.
- Server Host Key verify karna.
- Users ko authenticate karna.
- Password ya SSH Keys accept karna.
- Encrypted sessions establish karna.
- Remote user ka shell start karna.
- Authentication events ko logs mein record karna.

---

# 2. OpenSSH Server Configuration File

Main SSH Server configuration file hai:

```text
/etc/ssh/sshd_config
```

Ye file decide karti hai ke SSH Server kis tarah behave karega.

Is file mein mukhtalif settings hoti hain, jaise:

- SSH listening port
- Root login policy
- Password authentication
- Public-Key authentication
- Allowed users aur groups
- Login timeout
- Maximum authentication attempts
- Forwarding options

---

# SSH Client vs SSH Server Configuration

Server aur Client configuration files ko kabhi confuse mat karein.

| File | Purpose |
|------|---------|
| `/etc/ssh/sshd_config` | SSH Server configuration |
| `/etc/ssh/ssh_config` | System-wide SSH Client configuration |
| `~/.ssh/config` | Per-user SSH Client configuration |

Yahan:

```text
sshd_config
```

mein **d** ka matlab hai:

> **Daemon**

---

# 3. OpenSSH Server Package Check Karna

Rocky Linux, RHEL ya AlmaLinux par:

```bash
rpm -q openssh-server
```

Agar package install na ho to:

```bash
sudo dnf install -y openssh-server
```

---

# 4. SSH Service Check Karna

Current status check karein:

```bash
systemctl status sshd
```

Service active hai ya nahi:

```bash
systemctl is-active sshd
```

Boot ke waqt automatically start hoti hai ya nahi:

```bash
systemctl is-enabled sshd
```

---

# Service Ko Start Aur Enable Karna

Service start karein:

```bash
sudo systemctl start sshd
```

Boot par automatically enable karein:

```bash
sudo systemctl enable sshd
```

Ek hi command mein start aur enable karein:

```bash
sudo systemctl enable --now sshd
```

---

# 5. Current SSH Configuration Dekhna

Configuration file display karein:

```bash
sudo cat /etc/ssh/sshd_config
```

Page by page dekhne ke liye:

```bash
sudo less /etc/ssh/sshd_config
```

Configuration edit karne ke liye:

```bash
sudo vim /etc/ssh/sshd_config
```

---

# 6. Default Configuration Aur Security Hardening

Default SSH Server configuration bohot se systems ke liye design ki gayi hai.

Lekin production environments mein zyada security controls ki zarurat hoti hai.

Common hardening settings:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

In settings ka matlab hai:

- Direct root login ko disable karna.
- Remote password authentication ko disable karna.
- SSH Key-Based Authentication ko allow karna.

---
# 7. Direct Root Login Risky Kyun Hai?

Remote system se **root** user ke zariye direct login karna generally recommend nahi kiya jata.

Is ke chand aham security risks ye hain:

- Har Linux system mein `root` username pehle se hi mojood hota hai.
- Attacker ko sirf password guess karna hota hai.
- Root user ke paas unlimited privileges hoti hain.
- Agar root account compromise ho jaye to poora system compromise ho sakta hai.
- Multiple administrators aik hi root account share kar sakte hain.
- Auditing mushkil ho jati hai.
- Accountability kam ho jati hai.

---

# Root Username Predictable Hota Hai

Har standard Linux system mein superuser account ka naam hota hai:

```text
root
```

Attacker ko username dhoondhne ki zarurat nahi hoti.

Usay sirf ye guess karna hota hai:

```text
root password
```

Is wajah se brute-force attack attacker ke liye kaafi aasaan ho jata hai kyun ke unknown values kam reh jati hain.

---

# Root Ke Paas Unlimited Privileges Hoti Hain

`root` user ye tamam kaam kar sakta hai:

- Kisi bhi file ko modify kar sakta hai.
- System data delete kar sakta hai.
- User passwords change kar sakta hai.
- Services stop ya restart kar sakta hai.
- Software install ya remove kar sakta hai.
- Security settings modify kar sakta hai.
- Sensitive information access kar sakta hai.
- System ko shutdown ya reboot kar sakta hai.

Agar root account compromise ho jaye to attacker ko system par complete control mil sakta hai.

---

# 8. Accountability Aur Auditing Ka Masla

Misaal ke taur par agar 10 Linux Administrators aik hi root account use karte hain.

Logs mein sirf itna nazar aayega:

```text
root performed an action
```

Lekin ye maloom karna mushkil ho jayega ke asal mein kis administrator ne woh kaam kiya tha.

Isi ko accountability problem kehte hain.

---

# Behtar Administrative Workflow

Har administrator ko ye workflow follow karna chahiye:

1. Sab se pehle apne personal normal account se login kare.
2. Jab administrative privileges ki zarurat ho to `sudo` use kare.
3. Logs automatically record karein ke kis user ne command execute ki.

Example:

```text
nadeem login karta hai
        │
        ▼
nadeem sudo use karta hai
        │
        ▼
Command root privileges ke saath execute hoti hai
        │
        ▼
Logs mein nadeem ka naam record hota hai
```

Is tarah accountability maintain rehti hai.

---

# 9. Shared Root Login Ki Bajaye `sudo` Use Karein

Example:

```bash
ssh admin1@server
```

Us ke baad:

```bash
sudo systemctl restart httpd
```

System logs ye information record karte hain:

- Original user ka naam
- Kaunsi command execute hui
- Kis waqt execute hui
- Command successful hui ya fail hui

---

# sudo Logs Check Karna

Rocky Linux ya RHEL par:

```bash
sudo grep sudo /var/log/secure
```

Ya journal ke zariye:

```bash
sudo journalctl _COMM=sudo
```

---

# 10. `PermitRootLogin` Setting

`PermitRootLogin` decide karti hai ke root account SSH ke zariye login kar sakta hai ya nahi.

Ye setting is file mein hoti hai:

```text
/etc/ssh/sshd_config
```

Example:

```text
PermitRootLogin no
```

---

# Common `PermitRootLogin` Values

| Value | Matlab |
|------|---------|
| `yes` | Root allowed authentication methods ke saath login kar sakta hai |
| `no` | Root SSH login mukammal tor par disable hai |
| `prohibit-password` | Root password se login nahi kar sakta, lekin SSH key se kar sakta hai |
| `forced-commands-only` | Root sirf predefined forced commands ke liye login kar sakta hai |

---

# Recommended Settings

Sab se secure configuration:

```text
PermitRootLogin no
```

Agar automation ke liye root ki SSH keys use karni hon to:

```text
PermitRootLogin prohibit-password
```

Lekin aam tor par recommended workflow ye hai:

```text
Normal User Login
        │
        ▼
sudo
        │
        ▼
Administrative Command
```

Is tareeqe se security bhi improve hoti hai aur accountability bhi maintain rehti hai.

---

# 11. Effective Root Login Setting Check Karna

Command run karein:

```bash
sudo sshd -T | grep -i permitrootlogin
```

Example Output:

```text
permitrootlogin prohibit-password
```

`sshd -T` actual effective configuration display karta hai.

Ye sirf configuration file dekhne se zyada reliable tareeqa hai.

---

# 12. Direct Root SSH Login Disable Karna

Configuration file edit karein:

```bash
sudo vim /etc/ssh/sshd_config
```

Setting change karein:

```text
PermitRootLogin no
```

File save karein.

Configuration validate karein:

```bash
sudo sshd -t
```

Agar koi output na aaye to syntax generally theek hoti hai.

Us ke baad SSH service restart karein:

```bash
sudo systemctl restart sshd
```

---

# ⚠️ Apne Aap Ko Lock Out Hone Se Bachayein

Root login disable karne se pehle hamesha:

- Ek normal administrative user create karein.
- User ko `wheel` group mein add karein.
- SSH login test karein.
- Confirm karein ke `sudo` kaam kar raha hai.
- Testing ke dauran current root session band na karein.

---

# Administrative User Create Karna

Example:

```bash
sudo useradd admin1
sudo passwd admin1
sudo usermod -aG wheel admin1
```

Verify karein:

```bash
id admin1
```

SSH login test karein:

```bash
ssh admin1@server
```

Us ke baad check karein:

```bash
sudo whoami
```

Expected Output:

```text
root
```

Ye confirm karta hai ke normal user successfully sudo ke zariye administrative tasks perform kar sakta hai.

---
# 13. Password Authentication Disable Kyun Karein?

Password Authentication ko disable karna security ke liye bohot achi practice hai.

Password authentication in attacks ka shikaar ho sakti hai:

- Brute-force attacks
- Password guessing
- Credential reuse
- Weak passwords
- Password theft
- Phishing attacks
- Automated internet scanning

SSH Keys aam passwords ke muqable mein zyada secure hoti hain.

---

# Password Authentication Setting

SSH configuration mein is setting ka naam hai:

```text
PasswordAuthentication
```

Password login disable karne ke liye:

```text
PasswordAuthentication no
```

Is setting ke baad users password ke zariye SSH login nahi kar sakenge.

---

# 14. Public-Key Authentication Enable Karna

Public-Key Authentication ko control karne wali setting hai:

```text
PubkeyAuthentication yes
```

Aksar Linux distributions mein ye setting default se enabled hoti hai.

Check karne ke liye command:

```bash
sudo sshd -T | grep -i pubkeyauthentication
```

Expected Output:

```text
pubkeyauthentication yes
```

---

# 15. Key-Only Authentication Configuration

Production environments mein commonly ye configuration use ki jati hai:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

Is configuration ka matlab:

- Root user directly SSH login nahi kar sakta.
- SSH Key Authentication allowed hai.
- Password Authentication disable hai.

Ye configuration security ko kaafi improve karti hai.

---

# ⚠️ Password Disable Karne Se Pehle Keys Zaroor Test Karein

Kabhi bhi ye setting:

```text
PasswordAuthentication no
```

enable karne se pehle ye tamam cheezen verify karein:

- SSH Keys successfully configure ho chuki hon.
- Public key `authorized_keys` file mein mojood ho.
- File permissions bilkul sahi hon.
- SELinux contexts bhi sahi hon.
- Dusri SSH session successfully login kar rahi ho.

Agar bina test kiye password disable kar diya to system se lock out hone ka khatra hota hai.

---

# Safe Testing Procedure

Configuration apply karne se pehle hamesha ye procedure follow karein.

### Step 1

Current SSH session ko open rakhein.

### Step 2

Ek naya terminal kholein.

### Step 3

SSH Key-Based Login test karein.

### Step 4

Verify karein ke `sudo` bilkul sahi kaam kar raha hai.

### Step 5

SSH configuration validate karein.

### Step 6

`sshd` ko reload ya restart karein.

### Step 7

Ek aur nayi SSH session se login test karein.

### Step 8

Jab sab kuch successfully verify ho jaye tab purani SSH session close karein.

Ye production environments ki best practice hai.

---

# 16. SSH Configuration Validate Karna

Service restart karne se pehle hamesha ye command chalayein:

```bash
sudo sshd -t
```

Agar koi output nahi aati to aam tor par iska matlab hota hai:

```text
Configuration syntax is valid
```

Agar syntax mein koi masla ho to output kuch is tarah hogi:

```text
/etc/ssh/sshd_config line 45: Unsupported option
```

Aisi surat mein pehle error ko fix karein, us ke baad hi service restart karein.

---

# 17. SSH Service Restart Ya Reload Karna

Service restart karne ke liye:

```bash
sudo systemctl restart sshd
```

Agar sirf configuration dobara load karni ho aur service reload support karti ho:

```bash
sudo systemctl reload sshd
```

Status verify karne ke liye:

```bash
systemctl status sshd
```

---

# Restart Aur Reload Mein Farq

| Action | Matlab |
|---------|---------|
| Restart | SSH service ko poori tarah stop karke dobara start karta hai. |
| Reload | Service ko bina restart kiye configuration dobara read karwata hai. |

Reload zyada behtar hota hai kyun ke existing connections aam tor par disturb nahi hoti.

Lekin har service reload support nahi karti.

---

# 18. Effective SSH Configuration Dekhna

Current effective SSH configuration dekhne ke liye:

```bash
sudo sshd -T
```

Specific settings search karne ke liye:

```bash
sudo sshd -T | grep -Ei 'permitrootlogin|passwordauthentication|pubkeyauthentication'
```

Example Output:

```text
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
```

Ye command final effective configuration dikhati hai jo SSH daemon asal mein use kar raha hota hai.

---

# 19. Important SSH Server Settings

| Setting | Maqsad |
|---------|---------|
| `Port` | SSH kis port par listen karega |
| `PermitRootLogin` | Root SSH login ko control karta hai |
| `PasswordAuthentication` | Password login enable ya disable karta hai |
| `PubkeyAuthentication` | SSH Key Authentication enable karta hai |
| `MaxAuthTries` | Maximum login attempts define karta hai |
| `LoginGraceTime` | Login complete karne ka maximum waqt |
| `AllowUsers` | Sirf specified users ko allow karta hai |
| `AllowGroups` | Sirf specified groups ko allow karta hai |
| `DenyUsers` | Specified users ko block karta hai |
| `DenyGroups` | Specified groups ko block karta hai |
| `X11Forwarding` | X11 Forwarding control karta hai |
| `AllowTcpForwarding` | SSH Port Forwarding control karta hai |
| `ClientAliveInterval` | Idle client ko periodically check karta hai |
| `ClientAliveCountMax` | Kitni checks ke baad inactive client disconnect hoga |

---
# 20. SSH Port Change Karna

Default SSH port hoti hai:

```text
22
```

Agar aap security ke liye custom port use karna chahein to configuration file mein likhein:

```text
Port 2222
```

Port change karne ke baad sirf SSH configuration update karna kaafi nahi hota.

Aap ko neeche wali cheezen bhi update karni hongi:

- Firewall rules
- SELinux port labeling (agar SELinux Enforcing mode mein ho)
- SSH client commands
- Monitoring tools
- Documentation

---

# Custom Port Par Connect Karna

Agar SSH port change karke **2222** kar di hai to login is tarah hoga:

```bash
ssh -p 2222 user@server
```

---

# Firewall Mein Naya Port Allow Karna

Example:

```bash
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
```

Ye commands firewall mein naya SSH port permanently allow karti hain.

---

# SELinux Mein SSH Port Configure Karna

Agar SELinux Enforcing mode mein hai to pehle required package install karein:

```bash
sudo dnf install -y policycoreutils-python-utils
```

Naya SSH port add karein:

```bash
sudo semanage port -a -t ssh_port_t -p tcp 2222
```

Agar port pehle kisi aur type ke saath registered ho to modify karein:

```bash
sudo semanage port -m -t ssh_port_t -p tcp 2222
```

Verify karne ke liye:

```bash
sudo semanage port -l | grep ssh
```

---

# ⚠️ Sirf Port Change Karna Security Nahi Hai

SSH port change karna automated internet scans ko kuch had tak kam kar sakta hai, lekin ye complete security solution nahi hai.

Asal security in cheezon se aati hai:

- Strong Authentication
- Firewall Restrictions
- SSH Keys
- Regular Patching
- Monitoring
- Rate Limiting

Port change sirf ek additional security layer hai.

---

# 21. Sirf Specific Users Ko SSH Allow Karna

Agar aap sirf kuch users ko SSH login ki permission dena chahte hain to:

```text
AllowUsers admin1 devops1
```

Ab sirf ye users SSH ke zariye login kar sakenge.

Verify karne ke liye:

```bash
sudo sshd -T | grep -i allowusers
```

---

# Sirf Ek Group Ko SSH Allow Karna

Example:

```text
AllowGroups sshusers
```

Group create karein:

```bash
sudo groupadd sshusers
```

User ko group mein add karein:

```bash
sudo usermod -aG sshusers admin1
```

Verify karein:

```bash
id admin1
```

---

# 22. Authentication Attempts Limit Karna

Configuration:

```text
MaxAuthTries 3
```

Iska matlab:

Har SSH connection par user ko sirf **3 authentication attempts** milengi.

Verify:

```bash
sudo sshd -T | grep -i maxauthtries
```

---

# 23. Login Grace Time Configure Karna

Example:

```text
LoginGraceTime 30
```

Iska matlab:

User ke paas login complete karne ke liye **30 seconds** honge.

Verify:

```bash
sudo sshd -T | grep -i logingracetime
```

---

# 24. Inactive SSH Sessions Disconnect Karna

Example:

```text
ClientAliveInterval 300
ClientAliveCountMax 2
```

Matlab:

- Server har **300 seconds** baad client ko check karega.
- Agar client do martaba response na de to SSH session disconnect kar di jayegi.

Ye idle sessions ko automatically close kar deta hai.

---

# 25. Zarurat Na Ho To Features Disable Karein

Example:

```text
X11Forwarding no
AllowTcpForwarding no
AllowAgentForwarding no
PermitTunnel no
```

Agar users ko in features ki zarurat nahi hai to inhein disable kar dena security ko improve karta hai.

---

# 26. Configuration Drop-In Files Use Karna

Modern OpenSSH versions support karti hain:

```text
/etc/ssh/sshd_config.d/
```

Main configuration file edit karne ke bajaye aap alag hardening file bana sakte hain.

Example:

```bash
sudo vim /etc/ssh/sshd_config.d/99-hardening.conf
```

Us mein likhein:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
```

Ye tareeqa configuration management ko aasaan bana deta hai.

---

# Include Configuration Check Karna

Main configuration file mein aksar ye line hoti hai:

```text
Include /etc/ssh/sshd_config.d/*.conf
```

Check karne ke liye:

```bash
grep -i '^Include' /etc/ssh/sshd_config
```

---

# 27. Configuration Order Aur Effective Values

SSH configuration kai jagahon se aa sakti hai:

- Main configuration file
- Drop-in configuration files
- Match blocks
- Distribution defaults
- Command-line options

Is liye hamesha final effective configuration verify karein:

```bash
sudo sshd -T
```

Ye actual running configuration dikhata hai.

---

# 28. Match Blocks Samajhna

`Match` block sirf kuch specific users, groups, IP addresses ya hosts ke liye settings apply karta hai.

Example:

```text
Match User backup
    PasswordAuthentication no
    AllowTcpForwarding no
    X11Forwarding no
```

`Match` ke baad likhi hui tamam settings sirf us matching condition par apply hongi.

Ye tab tak continue rehti hain jab tak doosra `Match` block ya file ka end na aa jaye.

---

# 29. SSH Authentication Logs Dekhna

Rocky Linux ya RHEL par:

```bash
sudo tail -f /var/log/secure
```

Ya journal ke zariye:

```bash
sudo journalctl -u sshd
```

Real-time monitoring ke liye:

```bash
sudo journalctl -u sshd -f
```

---

# Common SSH Log Events

SSH logs mein aam tor par ye events nazar aate hain:

- Successful Password Login
- Successful Public-Key Login
- Failed Password Attempt
- Invalid User
- Root Login Reject Hua
- Client Disconnect Hua
- Key Authentication Failure
- Configuration Errors

Ye logs troubleshooting aur security auditing ke liye bohot important hote hain.

---
# 30. Root Login Restriction Test Karna

Kisi doosre system se root login test karein:

```bash
ssh root@server
```

Agar root login disable hai to expected output kuch is tarah hoga:

```text
Permission denied
```

Logs check karne ke liye:

```bash
sudo journalctl -u sshd -n 50
```

Is se aap verify kar sakte hain ke root login successfully block ho raha hai.

---

# 31. Password Authentication Restriction Test Karna

Password authentication ko force karne ke liye:

```bash
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no user@server
```

Agar `PasswordAuthentication no` configured hai to login fail ho jana chahiye.

Is se aap verify kar sakte hain ke password login successfully disable ho chuka hai.

---

# 32. Public-Key Authentication Test Karna

SSH Key Authentication test karne ke liye:

```bash
ssh -o PreferredAuthentications=publickey user@server
```

Debug mode mein chalane ke liye:

```bash
ssh -vv user@server
```

Output mein aap ko kuch is tarah ki lines nazar aayengi:

```text
Offering public key
Server accepts key
Authenticated using "publickey"
```

Ye confirm karta hai ke SSH Key Authentication successfully kaam kar rahi hai.

---

# 33. Recommended Baseline Configuration

Production environments ke liye aik common hardened configuration:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
X11Forwarding no
AllowAgentForwarding no
```

⚠️ Ye sirf ek sample configuration hai.

Isay apply karne se pehle apni organization ki requirements aur applications zaroor verify karein.

---

# 34. Complete SSH Hardening Workflow

```text
Current SSH Configuration Review Karein
              │
              ▼
Normal Administrative User Create Karein
              │
              ▼
SSH Key Authentication Configure Karein
              │
              ▼
Dusri SSH Session Mein Login Test Karein
              │
              ▼
Direct Root Login Disable Karein
              │
              ▼
Password Authentication Disable Karein
              │
              ▼
Configuration Ko sshd -t Se Validate Karein
              │
              ▼
sshd Ko Reload Ya Restart Karein
              │
              ▼
Dobara Test Karein Aur Logs Review Karein
```

Ye workflow production environments mein safest approach mana jata hai.

---

# 🧪 Practice Lab

## Step 1 – Configuration Ka Backup Banayein

```bash
sudo cp -a /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Verify karein:

```bash
ls -l /etc/ssh/sshd_config*
```

---

## Step 2 – Current Effective Configuration Check Karein

```bash
sudo sshd -T | grep -Ei 'permitrootlogin|passwordauthentication|pubkeyauthentication'
```

---

## Step 3 – Administrative User Create Karein

```bash
sudo useradd admin1
sudo passwd admin1
sudo usermod -aG wheel admin1
```

---

## Step 4 – admin1 Ke Liye SSH Keys Configure Karein

Client machine se:

```bash
ssh-copy-id admin1@server
```

Login test karein:

```bash
ssh admin1@server
```

---

## Step 5 – SSH Server Configuration Edit Karein

```bash
sudo vim /etc/ssh/sshd_config
```

Settings configure karein:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

---

## Step 6 – Configuration Validate Karein

```bash
sudo sshd -t
```

---

## Step 7 – SSH Service Restart Karein

```bash
sudo systemctl restart sshd
```

---

## Step 8 – Nayi Terminal Se Login Test Karein

```bash
ssh admin1@server
```

Phir check karein:

```bash
sudo whoami
```

Expected Output:

```text
root
```

---

## Step 9 – Root Login Test Karein

```bash
ssh root@server
```

Expected result:

Root login deny ho jana chahiye.

---

## Step 10 – SSH Logs Review Karein

```bash
sudo journalctl -u sshd -n 50
```

---

# 🔧 Troubleshooting Scenarios

## Scenario 1 – sshd Restart Nahi Ho Raha

Syntax check karein:

```bash
sudo sshd -t
```

Service status dekhein:

```bash
sudo systemctl status sshd
```

Logs dekhein:

```bash
sudo journalctl -u sshd -n 100
```

Agar zarurat ho to backup restore karein:

```bash
sudo cp -a /etc/ssh/sshd_config.backup /etc/ssh/sshd_config
```

---

## Scenario 2 – SSH Key Login Kaam Nahi Kar Raha

Client side debugging:

```bash
ssh -vv user@server
```

Server logs:

```bash
sudo journalctl -u sshd -n 50
```

Permissions verify karein:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

SELinux context restore karein:

```bash
sudo restorecon -Rv ~/.ssh
```

---

## Scenario 3 – Root Abhi Bhi Login Kar Raha Hai

Effective configuration check karein:

```bash
sudo sshd -T | grep -i permitrootlogin
```

Saari SSH configuration files search karein:

```bash
sudo grep -Rni 'PermitRootLogin' /etc/ssh/
```

Ho sakta hai koi Drop-in file ya Match block original configuration ko override kar raha ho.

---

## Scenario 4 – Password Login Abhi Bhi Kaam Kar Raha Hai

Check karein:

```bash
sudo sshd -T | grep -i passwordauthentication
```

Configuration search karein:

```bash
sudo grep -Rni 'PasswordAuthentication' /etc/ssh/
```

Saath hi ye settings bhi check karein:

```text
KbdInteractiveAuthentication
AuthenticationMethods
```

Kuch systems keyboard-interactive authentication separately allow karte hain.

---

## Scenario 5 – Configuration Change Ke Baad Lock Out Ho Gaye

Agar SSH access band ho jaye to ye options use karein:

- Hypervisor Console
- Physical Console
- Rescue Mode
- Cloud Serial Console
- Existing Open SSH Session

Purani configuration restore karein aur `sshd` restart karein.

---

## Scenario 6 – Custom SSH Port Kaam Nahi Kar Raha

Check karein SSH kis port par listen kar raha hai:

```bash
sudo ss -tulpn | grep sshd
```

Firewall verify karein:

```bash
sudo firewall-cmd --list-all
```

SELinux verify karein:

```bash
sudo semanage port -l | grep ssh
```

Local testing:

```bash
ssh -p 2222 localhost
```

---

# ⚠️ Security Best Practices

Hamesha in best practices ko follow karein:

- Direct Root Login disable karein.
- Personal administrative accounts use karein.
- Elevated privileges ke liye `sudo` use karein.
- Passwords ki bajaye SSH Keys prefer karein.
- Administrator keys par passphrase zaroor lagayein.
- `AllowUsers` ya `AllowGroups` use karein.
- SSH logs regularly review karein.
- OpenSSH ko hamesha updated rakhein.
- Firewall access trusted networks tak limited rakhein.
- `MaxAuthTries` configure karein.
- Non-required forwarding features disable karein.
- Configuration ka backup zaroor banayein.
- Restart se pehle `sshd -t` zaroor chalayein.
- Current session band karne se pehle doosri SSH session se testing zaroor karein.

---

# 📌 Quick Revision

| Item | Matlab |
|------|---------|
| `sshd` | OpenSSH Server Daemon |
| `/etc/ssh/sshd_config` | Main SSH Server Configuration File |
| `PermitRootLogin no` | Direct Root Login Disable |
| `PasswordAuthentication no` | Password Login Disable |
| `PubkeyAuthentication yes` | SSH Key Authentication Enable |
| `sshd -t` | Configuration Validate Kare |
| `sshd -T` | Effective Configuration Show Kare |
| `systemctl restart sshd` | SSH Service Restart Kare |
| `AllowUsers` | Sirf Specified Users Ko Allow Kare |
| `AllowGroups` | Sirf Specified Groups Ko Allow Kare |

---

# Common Commands

| Command | Kaam |
|---------|------|
| `systemctl status sshd` | SSH Service Status Check Kare |
| `systemctl enable --now sshd` | SSH Enable Aur Start Kare |
| `sudo vim /etc/ssh/sshd_config` | SSH Configuration Edit Kare |
| `sudo sshd -t` | Configuration Validate Kare |
| `sudo sshd -T` | Effective Configuration Show Kare |
| `sudo systemctl restart sshd` | SSH Restart Kare |
| `sudo journalctl -u sshd` | SSH Logs Dekhein |
| `sudo tail -f /var/log/secure` | Authentication Logs Live Dekhein |
| `sudo ss -tulpn \| grep sshd` | SSH Listening Port Check Kare |
| `ssh -vv user@server` | SSH Client Debug Kare |

---

# 📖 Key Takeaways

- OpenSSH Server service `sshd` daemon provide karta hai.
- Main configuration file `/etc/ssh/sshd_config` hoti hai.
- Direct Root Login security aur accountability dono ke liye risk hai.
- Administrators ko personal accounts se login karke `sudo` use karna chahiye.
- SSH Keys passwords se zyada secure hoti hain.
- `PermitRootLogin` Root SSH access control karta hai.
- `PasswordAuthentication` Password Login control karta hai.
- `PubkeyAuthentication` SSH Key Login control karta hai.
- Restart se pehle hamesha `sshd -t` chalayein.
- Configuration changes ko hamesha doosri SSH session se verify karein.

---

# 💡 Yaad Rakhein

> **SSH Server Configuration ko apne Linux system ke main security gate ki tarah samjhein.**
>
> - `PermitRootLogin` decide karta hai ke Root seedha andar aa sakta hai ya nahi.
> - `PasswordAuthentication` decide karta hai ke password accept hoga ya nahi.
> - `PubkeyAuthentication` decide karta hai ke SSH Keys accept hongi ya nahi.
> - `AllowUsers` aur `AllowGroups` decide karte hain ke kaun login kar sakta hai.
>
> **Recommended Administrative Workflow:**
>
> ```text
> Personal User Account
>          │
>          ▼
> Secure SSH Key Login
>          │
>          ▼
> sudo
>          │
>          ▼
> Administrative Task
> ```
>
> **Golden Rule:**
>
> **Kabhi bhi apna current login method disable mat karein jab tak naya aur secure login method successfully test na ho jaye.**
