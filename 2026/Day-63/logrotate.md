# MODULE 08 – Logrotate & System Logging Notes
> **Analyze and Store Logs**

---

# 📖 Introduction

Is section mein hum Linux ke **system logging** ko samjhenge aur dekhenge ke operating system logs ko kis tarah collect, process aur store karta hai.

Hum neeche diye gaye topics cover karenge:

- System Logging
- Log Events
- Log Rotation (Logrotate)
- Syslog Analysis
- Journal Logs
- Journal Storage
- Accurate System Time

---

# 1. What is System Logging?

Jab Linux system chal raha hota hai to Operating System, Kernel aur applications har important activity ka record banate rehte hain.

Is record ko **System Logs** kehte hain.

Ye logs administrator ko system monitor karne, audit karne aur problems troubleshoot karne mein madad karte hain.

---

# 2. Why Are Logs Important?

Agar users complain karein ke:

- System slow hai
- Login mein delay aa raha hai
- Application crash ho rahi hai
- Service start nahi ho rahi
- CPU ya Memory high use ho rahi hai

To System Administrator ka pehla kaam hota hai:

✅ Logs ko check karna.

Logs ki madad se administrator issue ki asal wajah tak pohanch sakta hai.

---

# 3. Logs Kis Cheez Ka Record Rakhte Hain?

System logs mein bohot si information hoti hai, jaise:

- User login/logout
- Failed login attempts
- Service start aur stop
- Kernel messages
- Hardware errors
- Disk errors
- CPU aur Memory issues
- Application events
- Cron jobs
- Boot messages

---

# 4. Linux Logs Kaise Dekhein?

Linux mein logs ko dekhne ke liye commonly ye commands use hoti hain:

```bash
less
```

Aur

```bash
tail
```

Example:

```bash
less /var/log/messages
```

```bash
tail /var/log/messages
```

---

# 5. Red Hat Mein Logs Ko Kaun Manage Karta Hai?

Red Hat based systems mein logging normally **do services** mil kar handle karti hain.

| Service | Kaam |
|----------|------|
| systemd-journald | Logs collect karta hai |
| rsyslog | Logs ko process aur store karta hai |

> **Interview Question**

**Question:** Linux mein logs ko manage karne wali services kaunsi hain?

**Answer:**

- systemd-journald
- rsyslog

---

# 6. systemd-journald Kya Hai?

`systemd-journald` Linux ki primary logging service hai.

Ye mukhtalif sources se logs collect karti hai.

---

## systemd-journald Kis Kis Jagah Se Logs Collect Karta Hai?

Ye service logs collect karti hai:

- Linux Kernel
- Early Boot Process
- Daemons
- Standard Output (stdout)
- Standard Error (stderr)
- Syslog Messages

---

# 7. Journal Logs Kahan Store Hote Hain?

Default installation mein journal logs temporary filesystem mein store hote hain.

Location:

```text
/run
```

Ye storage:

- Temporary hoti hai
- RAM based hoti hai
- Reboot ke baad delete ho jati hai

Is liye isay **Non-Persistent Journal Storage** kehte hain.

---

# 8. rsyslog Kya Hai?

`rsyslog` ek service hai jo:

- journald se logs receive karti hai
- unko process karti hai
- unko appropriate log files mein save karti hai
- zarurat ho to remote server ko forward bhi karti hai

---

# 9. rsyslog Ka Kaam

rsyslog:

- Journal se logs read karta hai.
- Logs ko filter karta hai.
- Priority ke mutabiq organize karta hai.
- Correct log file mein likhta hai.
- Remote logging bhi support karta hai.

Agar rsyslog service na ho to:

- Logs permanent files mein save nahi honge.
- Troubleshooting mushkil ho jayegi.

---

# 10. Permanent Log Files

rsyslog logs ko permanently save karta hai.

Location:

```text
/var/log
```

Ye directory reboot ke baad bhi data ko preserve karti hai.

---

# 11. Common Log Files

| File | Purpose |
|------|---------|
| /var/log/messages | General system messages |
| /var/log/secure | Authentication aur security logs |
| /var/log/maillog | Mail server logs |
| /var/log/cron | Cron jobs ke logs |
| /var/log/boot.log | Boot process logs |

---

# 12. Logging Flow

```text
Applications
        │
        ▼
Linux Kernel
        │
        ▼
systemd-journald
        │
        ▼
rsyslog
        │
        ▼
/var/log/
        │
        ▼
Different Log Files
```

---

# 13. What is Logrotate?

System continuously naye logs generate karta rehta hai.

Agar logs ko manage na kiya jaye to:

- Disk bhar sakti hai
- Performance slow ho sakti hai
- Important logs dhoondhna mushkil ho jata hai

Is problem ka solution hai:

## Logrotate

---

# 14. Logrotate Kya Hai?

Logrotate ek Linux utility hai jo:

- Purane logs ko archive karti hai
- Naye log files create karti hai
- Old logs ko compress karti hai
- Zarurat par old logs delete karti hai

Ye automatically scheduled hota hai.

---

# 15. Log Rotation Kyun Zaroori Hai?

Log rotation ki wajah se:

- Disk space bachti hai
- System fast rehta hai
- Logs organized rehte hain
- Old logs archive ho jate hain
- Backup lena aasaan ho jata hai

---

# 16. Logrotate Ka Typical Process

```text
Current Log File
        │
        ▼
Log File Full
        │
        ▼
Rename Old Log
        │
        ▼
Create New Empty Log
        │
        ▼
Compress Old Log
        │
        ▼
Delete Very Old Logs
```

---

# 17. Administrator Ki Responsibility

Ek Linux Administrator ko:

- Logs monitor karne chahiye.
- Errors identify karni chahiye.
- Security events review karne chahiye.
- Disk usage monitor karni chahiye.
- Log rotation verify karni chahiye.

---

# 18. Important Interview Questions

### Q1. Linux mein logging ko kaunsi services manage karti hain?

**Answer**

- systemd-journald
- rsyslog

---

### Q2. systemd-journald ka kaam kya hai?

Logs collect karta hai different sources se.

---

### Q3. rsyslog ka kaam kya hai?

Logs ko process karke `/var/log` mein permanently save karta hai.

---

### Q4. Journal logs default kahan store hote hain?

```text
/run
```

---

### Q5. Permanent logs kis directory mein hote hain?

```text
/var/log
```

---

### Q6. Logrotate ka purpose kya hai?

Old log files ko:

- Rotate
- Compress
- Archive
- Delete

karna.

---

# 19. Important Commands

View filesystem:

```bash
df -h
```

View log file:

```bash
less /var/log/messages
```

Last few lines:

```bash
tail /var/log/messages
```

Journal logs:

```bash
journalctl
```

---

# 20. Quick Revision

| Topic | Yaad Rakhna Hai |
|--------|-----------------|
| Temporary journal | `/run` |
| Permanent logs | `/var/log` |
| Log collector | `systemd-journald` |
| Log processor | `rsyslog` |
| Log rotation tool | `logrotate` |
| General log file | `/var/log/messages` |
| Security logs | `/var/log/secure` |
| Cron logs | `/var/log/cron` |
| Mail logs | `/var/log/maillog` |
| Boot logs | `/var/log/boot.log` |

---

# 💡 Yaad Rakhein

> **Ek achha Linux System Administrator sirf commands nahi chalata, balki logs ko analyze karke problems ki asal wajah tak pohanchta hai. Logs troubleshooting ka sab se powerful tool hain.**