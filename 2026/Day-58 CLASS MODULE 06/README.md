# Module 06 - Linux System Administration
# DIN 58
# Systemd, Logging, /var, Journalctl, Rsyslog, Logrotate, Backup aur Cron

> TAREEKH JULY 4TH, 2026
> **NIT Academy**
>
> Mastering Linux Bootcamp
>
> Module 06 - Roman Urdu Learning Notes

---

# Learning Objectives

Is module ke baad students ko yeh concepts achi tarah samajh aane chahiye:

- Systemd kya hai
- PID 1 ki importance
- Linux logging architecture
- /var directory ka purpose
- journalctl ka istemal
- rsyslog ka role
- Logrotate ka kaam
- Retention Policy kya hoti hai
- Companies logs ka backup kyun leti hain
- Cron automation ka basic concept

---

# Chapter 1 - Systemd

## Systemd kya hai?

Systemd Linux ka **System aur Service Manager** hai.

Iska kaam hai:

- Linux ko boot karna
- Services start karna
- Services manage karna
- Timers manage karna
- Mount points handle karna
- Hardware devices manage karna

Systemd Linux ka pehla userspace process hota hai.

```text
PID 1
```

Yani Linux ke boot hone ke baad sab se pehla process Systemd hota hai.

Uske baad baaqi tamam services isi ke through start hoti hain.

---

# Linux Boot Process

```text
BIOS / UEFI

↓

GRUB

↓

Linux Kernel

↓

systemd (PID 1)

↓

sshd
Network
Cron
Apache
Docker
MariaDB
Cloudflared
```

Is liye agar Systemd start na ho to Linux properly operate nahi karega.

---

# Systemd Units

Systemd har cheez ko ek **Unit** ki form mein manage karta hai.

Common Unit Types

| Unit | Kaam |
|------|------|
| Service | Programs chalana |
| Target | Services ka group |
| Timer | Scheduled jobs |
| Mount | Filesystem mount karna |
| Device | Hardware manage karna |
| Socket | Network sockets |

Examples

```text
sshd.service

docker.service

NetworkManager.service

logrotate.timer

multi-user.target
```

---

# systemctl

Linux administrator services ko manage karne ke liye:

```bash
systemctl
```

command use karta hai.

Examples

Service Start

```bash
systemctl start httpd
```

Service Stop

```bash
systemctl stop httpd
```

Restart

```bash
systemctl restart httpd
```

Reload Configuration

```bash
systemctl reload httpd
```

Boot par Start

```bash
systemctl enable httpd
```

Disable

```bash
systemctl disable httpd
```

Status

```bash
systemctl status httpd
```

---

# Important Systemd Directories

| Directory | Purpose |
|------------|----------|
| /usr/lib/systemd/system | Vendor ke unit files |
| /run/systemd/system | Runtime unit files |
| /etc/systemd/system | Administrator ke custom files |

Priority

```text
Highest

/etc/systemd/system

↓

/run/systemd/system

↓

/usr/lib/systemd/system

Lowest
```

---

# Chapter 2 - /var Directory

`/var` ka matlab hai:

> Variable

Yahan wo files store hoti hain jo continuously change hoti rehti hain.

Examples:

- Logs
- Mail
- Cache
- Database Data
- Temporary Files
- Print Queue

---

# Important /var Directories

| Directory | Purpose |
|------------|----------|
| /var/log | Logs |
| /var/lib | Application Data |
| /var/cache | Cache Files |
| /var/tmp | Temporary Files |
| /var/spool | Waiting Jobs |

---

# /var itna Important Kyun Hai?

Almost har Linux service apna data `/var` mein store karti hai.

Examples

Apache

```text
/var/log/httpd
```

MariaDB

```text
/var/lib/mysql
```

DNF

```text
/var/cache/dnf
```

Cron

```text
/var/spool
```

---

# Agar /var Full Ho Jaye To?

Agar `/var` ki storage bhar jaye to:

- Logs likhna band ho jata hai
- Database crash ho sakti hai
- Package install fail ho jate hain
- Web server stop ho sakta hai
- Multiple services fail ho sakti hain

Disk usage check karne ke liye:

```bash
df -h
```

---

# Chapter 3 - Linux Logging

Linux har waqt apni activities record karta rehta hai.

Is record ko:

```text
Log
```

kehte hain.

Examples

- Login attempts
- SSH connections
- Kernel Messages
- Cron Jobs
- Firewall Events
- Software Installations

---

# Logs Kyun Important Hain?

Logs hume batate hain:

- Kya hua?
- Kab hua?
- Kis ne kiya?
- Error kyun ayi?

System Administrator ka sab se important troubleshooting tool logs hi hote hain.

---

# Linux Logging Architecture

```text
Applications

↓

systemd-journald

↓

rsyslog

↓

/var/log
```

---

# systemd-journald

Systemd-journald ka kaam hai:

- Logs collect karna
- Binary format mein save karna

Isko yaad rakhein:

> Collector

---

# journalctl

journalctl ek command hai jo binary logs ko read karti hai.

Useful Commands

Sab logs

```bash
journalctl
```

Last 20 entries

```bash
journalctl -n 20
```

Live logs

```bash
journalctl -f
```

Last one hour

```bash
journalctl --since "1 hour ago"
```

SSHD logs

```bash
journalctl -u sshd
```

Cloudflared

```bash
journalctl -u cloudflared
```

Without Pager

```bash
journalctl -u cloudflared -n 50 --no-pager
```

---

# Pager Kya Hota Hai?

Normally journalctl output ko:

```text
less
```

program ke andar kholta hai.

Isko pager kehte hain.

Agar hum:

```bash
--no-pager
```

likhein to output seedha terminal par print hoti hai.

Yeh automation aur scripting mein bohat useful hai.

---

# Live Monitoring

Terminal 1

```bash
journalctl -f
```

Terminal 2

```bash
logger "Linux Logging Test"
```

Student foran live log dekh sakta hai.

---

# rsyslog

Rsyslog ko yaad rakhein:

> Text Writer

Yeh logs ko text files mein likhta hai.

Examples

```text
/var/log/messages

/var/log/secure

/var/log/cron

/var/log/maillog
```

Configuration

```text
/etc/rsyslog.conf
```

Extra configuration

```text
/etc/rsyslog.d/
```

---

# Useful Commands

Logs dekhna

```bash
tail -f /var/log/messages
```

Manual log banana

```bash
logger "Hello Linux"
```

---

# journalctl vs rsyslog

| journalctl | rsyslog |
|------------|----------|
| Binary Database | Plain Text |
| Fast Search | Sequential Search |
| Advanced Filtering | Basic Search |

---

# Chapter 4 - SIEM

SIEM ka matlab hai:

```text
Security Information and Event Management
```

Enterprise companies tamam servers ke logs aik central jagah par bhejti hain.

Us system ko SIEM kehte hain.

Kaam:

- Centralized Logging
- Real-Time Alerts
- Security Monitoring
- Compliance
- Long-Term Storage

Popular SIEM

- Splunk
- ELK
- Microsoft Sentinel
- Wazuh

---

# Chapter 5 - Logrotate

## Logrotate Kyun Zaroori Hai?

Logs continuously barhte rehte hain.

Agar unko manage na kiya jaye to:

```text
messages

↓

10 GB

↓

100 GB

↓

500 GB

↓

Disk Full
```

---

# Logrotate Kya Karta Hai?

Automatically:

- Log Rotate karta hai
- Compress karta hai
- Purane logs delete karta hai
- Naya log create karta hai

---

# Configuration Files

Global

```text
/etc/logrotate.conf
```

Service Specific

```text
/etc/logrotate.d/
```

---

# Example Configuration

```text
weekly

rotate 4

create

dateext

compress
```

---

# Meaning

## weekly

Har haftay rotate hoga.

## rotate 4

Sirf 4 archives rakho.

## create

Naya empty log banao.

## dateext

Date append karo.

## compress

Purane logs ko zip karo.

---

# Retention Policy

```text
Week 1

app.log.1

Week 2

app.log.1

app.log.2

Week 3

app.log.1

app.log.2

app.log.3

Week 4

app.log.1

app.log.2

app.log.3

app.log.4

Week 5

Oldest delete

New archive create
```

Yani sirf latest 4 weeks ka data rakha jayega.

---

# Original File Delete Kyun Hoti Hai?

Rotation ke baad:

```text
messages

↓

messages-20260701

↓

messages-20260701.gz
```

Original file delete kar di jati hai kyun ke compressed file usi data ko contain karti hai aur disk space bachti hai.

---

# Logrotate Timer

Check timer

```bash
systemctl list-timers
```

Look for

```text
logrotate.timer
```

---

# Logrotate Status

```bash
cat /var/lib/logrotate/logrotate.status
```

Is se pata chalta hai:

- Kis log ko kab rotate kiya gaya
- Last rotation date
- Tracking information

---

# Test Log

```bash
cat << EOF >> /var/log/myapp.log

User Login

User Logout

EOF
```

---

# Test Logrotate

Dry Run

```bash
logrotate -d /etc/logrotate.conf
```

Force Rotation

```bash
logrotate -f /etc/logrotate.conf
```

---

# Chapter 6 - Backup

Companies logs ko archive karti hain.

Typical Flow

```text
Applications

↓

Logs

↓

Logrotate

↓

Compressed Logs

↓

Backup Server

↓

Long-Term Storage
```

---

# NFS Kyun Use Karte Hain?

Production server ke compressed logs aksar NFS server par copy kiye jate hain.

Benefits

- Disk space bachti hai
- Central backup hota hai
- Disaster Recovery easy hoti hai
- Compliance maintain hoti hai

---

# Chapter 7 - Cron

Cron Linux ka scheduler hai.

Automatically commands run karta hai.

Examples

- Backup
- Cleanup
- Reports
- Logrotate
- Monitoring Scripts

Commands

Edit

```bash
crontab -e
```

List

```bash
crontab -l
```

System Jobs

```bash
ls /etc/cron.daily
```

---

# Production Logging Workflow

```text
Applications

↓

systemd-journald

↓

rsyslog

↓

/var/log

↓

logrotate

↓

Compressed Logs

↓

NFS / NAS / Cloud Backup

↓

SIEM

↓

Security Monitoring
```

---

# Key Takeaways

- Systemd Linux ka pehla process hai (PID 1).
- `/var` variable data store karta hai.
- systemd-journald logs collect karta hai.
- journalctl binary logs read karta hai.
- rsyslog text files likhta hai.
- logrotate disk ko bharne se bachata hai.
- Retention Policy decide karti hai kitne logs rakhne hain.
- Companies logs ko backup aur archive karti hain.
- Cron repetitive kaam automatically perform karta hai.
- SIEM enterprise level monitoring ke liye use hota hai.

---

# End of Module 06

**Mastering Linux**

**NIT Academy**