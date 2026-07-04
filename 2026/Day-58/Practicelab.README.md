# Linux Practical Lab
# Module 06 - Systemd, Logging, Journalctl, Logrotate aur Cron

> **NIT Academy**
>
> **Mastering Linux Bootcamp**
>
> **Estimated Time:** 2.5 – 3 Ghantay
>
> **Difficulty:** ⭐⭐⭐☆☆ (Intermediate)

---

# Lab Objectives

Is practical lab ke baad aap:

- `/var` directory ko explore kar sakenge.
- Linux logging ko samajh sakenge.
- `/var/log` directory ko explore karenge.
- `tail` command se live logs monitor karenge.
- Apni custom log entries generate karenge.
- `journalctl` use karna seekhenge.
- Real-time log monitoring karenge.
- `journalctl` aur `rsyslog` ka difference samjhenge.
- Logrotate ko samjhenge.
- Logrotate ko manually execute karenge.
- Compression aur Retention Policy ko samjhenge.
- Cron aur Systemd Timers ka basic concept samjhenge.

---

# Lab 1 - Exploring the /var Directory

## Task 1

`/var` directory ke contents display karein.

```bash
ls /var
```

### Questions

1. `/var` ka matlab kya hai?
2. Logs kis directory mein store hotay hain?
3. Cache kis directory mein store hoti hai?
4. Applications ka runtime data kis directory mein hota hai?
5. Cron jobs kis directory mein milti hain?

---

## Task 2

Disk usage check karein.

```bash
df -h
```

### Questions

1. Agar `/var` full ho jaye to kya problems aa sakti hain?
2. Konsi Linux services fail ho sakti hain?

---

# Lab 2 - Exploring Linux Logs

Log files display karein.

```bash
ls -lh /var/log
```

Neeche diye gaye files locate karein.

- messages
- secure
- cron
- maillog
- lastlog
- wtmp

### Questions

Kaunsa file store karta hai:

- General system messages?
- Authentication logs?
- Cron jobs?
- Mail logs?

---

# Lab 3 - Exploring rsyslog

Global rsyslog configuration dekhein.

```bash
cat /etc/rsyslog.conf
```

Additional configuration files dekhein.

```bash
ls /etc/rsyslog.d
```

### Questions

1. Linux service configurations ko separate files mein kyun rakhta hai?
2. Global configuration file ka naam kya hai?

---

# Lab 4 - Watching Logs in Real Time

### Terminal 1

```bash
tail -f /var/log/messages
```

### Terminal 2

Ek manual log create karein.

```bash
logger "Student Lab - Testing Linux Logging"
```

Observe karein ke log live show hota hai.

### Questions

1. Log kis file mein aya?
2. `logger` command kya karti hai?
3. `tail -f` administrator ke liye kyun useful hai?

---

# Lab 5 - Exploring journalctl

Sab logs dekhein.

```bash
journalctl
```

Last 20 entries dekhein.

```bash
journalctl -n 20
```

Last 1 minute ke logs.

```bash
journalctl --since "1 minute ago"
```

SSH service ke logs.

```bash
journalctl -u sshd
```

Cloudflared ke logs.

```bash
journalctl -u cloudflared -n 50 --no-pager
```

### Questions

1. `-u` ka matlab kya hai?
2. `-n` kya karta hai?
3. `--no-pager` kyun use karte hain?
4. Pager kya hota hai?

---

# Lab 6 - Watching journalctl Live

### Terminal 1

```bash
journalctl -f
```

### Terminal 2

Ek aur log generate karein.

```bash
logger "Testing journalctl live monitoring"
```

Observe karein ke log foran display hota hai.

### Questions

1. `tail -f` aur `journalctl -f` mein kya difference hai?
2. Systemd services monitor karne ke liye konsa command best hai?

---

# Lab 7 - Exploring Logrotate

Global configuration file dekhein.

```bash
cat /etc/logrotate.conf
```

Neeche diye gaye directives identify karein.

- weekly
- rotate
- create
- dateext
- compress
- include

### Questions

Har directive ka purpose explain karein.

---

# Lab 8 - Exploring Service-Specific Logrotate Files

Directory list karein.

```bash
ls -l /etc/logrotate.d
```

Rsyslog configuration dekhein.

```bash
cat /etc/logrotate.d/rsyslog
```

Chrony configuration dekhein.

```bash
cat /etc/logrotate.d/chrony
```

### Questions

1. Har application ki alag configuration kyun hoti hai?
2. Kaunsi configuration global hoti hai?

---

# Lab 9 - Understanding the Logrotate Timer

Systemd timers display karein.

```bash
systemctl list-timers
```

Logrotate timer locate karein.

```bash
systemctl list-timers | grep logrotate
```

### Questions

1. Timer ka naam kya hai?
2. Yeh kis service ko execute karta hai?
3. Systemd timer ka kya faida hai?

---

# Lab 10 - Exploring Logrotate Status

Status file display karein.

```bash
cat /var/lib/logrotate/logrotate.status
```

### Questions

1. Last rotation kab hui?
2. Konsi log files tracked hain?
3. Linux is file ko maintain kyun karta hai?

---

# Lab 11 - Creating Your Own Log File

Custom application log create karein.

```bash
cat << EOF >> /var/log/myapp.log
User Login
Dashboard Opened
Downloaded Report
User Logout
EOF
```

Verify karein.

```bash
cat /var/log/myapp.log
```

File information dekhein.

```bash
ls -lh /var/log/myapp.log
```

### Questions

1. Applications logs kyun generate karti hain?
2. `logger` aur manually file create karne mein kya difference hai?

---

# Lab 12 - Testing Logrotate

Dry run karein.

```bash
logrotate -d /etc/logrotate.conf
```

Force execute karein.

```bash
logrotate -f /etc/logrotate.conf
```

Logs verify karein.

```bash
ls -lh /var/log
```

### Questions

1. Kya nayi log file create hui?
2. Purani file rename hui?
3. Kya koi warning ayi?

---

# Lab 13 - Compression

Compressed files locate karein.

```bash
ls -lh /var/log
```

`.gz` files identify karein.

### Questions

1. Logs compress kyun kiye jate hain?
2. Original file delete kyun hoti hai?
3. Compression ke kya benefits hain?

---

# Lab 14 - Understanding Retention Policy

Neeche di gayi configuration ko samjhein.

```text
weekly
rotate 4
```

### Questions

1. Kitne hafton ke logs retain honge?
2. Agar configuration ho:

```text
daily
rotate 4
```

Kitne din ka backup rahega?

3. Agar configuration ho:

```text
monthly
rotate 6
```

Kitne mahino ka backup rahega?

---

# Lab 15 - Exploring Cron

Daily cron jobs dekhein.

```bash
ls /etc/cron.daily
```

Apni crontab dekhein.

```bash
crontab -l
```

Ek cron job banayein jo har minute current date ko ek file mein likhe.

Verify karein ke cron successfully run hua.

### Questions

1. Cron ka purpose kya hai?
2. Companies Cron se kaun kaun se kaam automate karti hain?

---

# RHCSA Challenge

Apni notes use kiye baghair:

- SSH logs display karein.
- Last 50 journal entries dekhein.
- Last 1 hour ke logs dekhein.
- Live logging monitor karein.
- Ek custom log generate karein.
- Logrotate configuration dekhein.
- Logrotate timer locate karein.
- Logrotate status file dekhein.
- Logrotate ko force execute karein.
- Compressed logs verify karein.
- Retention Policy explain karein.

---

# Enterprise Challenge

Aap company ke Linux Administrator hain.

Developer complain karta hai:

> "Application logs generate nahi kar rahi."

Neeche diye gaye steps perform karein.

1. Disk usage check karein.
2. Verify karein ke `/var` full nahi hai.
3. `/var/log` inspect karein.
4. journalctl check karein.
5. rsyslog verify karein.
6. Logrotate configuration inspect karein.
7. Logrotate timer verify karein.
8. Check karein ke logs successfully rotate huye hain ya nahi.
9. Permanent solution recommend karein.

---

# Reflection Questions

1. journalctl aur rsyslog mein kya difference hai?
2. `journalctl -f` aur `tail -f` mein kya farq hai?
3. Linux logs rotate kyun karta hai?
4. `/var` itni important directory kyun hai?
5. Agar Logrotate kaam karna band kar de to kya hoga?
6. Troubleshooting mein logs ka role kya hota hai?
7. Companies logs ko central server par kyun bhejti hain?
8. Historical logs maintain karne ka kya faida hai?

---

# Mubarak Ho!

Aap ne successfully Module 06 ka practical lab complete kar liya.

Aap ne practice ki:

- Systemd
- Linux Logging
- `/var`
- rsyslog
- journalctl
- Logrotate
- Compression
- Retention Policies
- Systemd Timers
- Cron
- Enterprise Troubleshooting

Ye tamam skills ek **Linux System Administrator**, **RHCSA**, **DevOps Engineer**, **SRE**, aur **Platform Engineer** ki rozana ki responsibilities ka hissa hoti hain.

---

# NIT Academy

## Mastering Linux Bootcamp