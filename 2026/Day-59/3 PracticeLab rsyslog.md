# MODULE 06/07 – Practice Lab Session: Rsyslog Configuration
> **Hands-on Practice Lab (Roman Urdu)**

---

# 🎯 Lab Ka Maqsad

Is practice lab mein aap seekhenge ke:

- `rsyslog` service ko verify kaise karna hai.
- rsyslog configuration file kahan mojood hoti hai.
- Default logging rules ko samajhna.
- Facility aur Priority kis tarah mil kar kaam karti hain.
- Mukhtalif log messages kis file mein save hote hain.
- rsyslog ko configure karke logs ko kisi specific file mein bhejna.

---

# 📋 Lab Requirements

Lab shuru karne se pehle ensure karein ke aap ke paas:

- Rocky Linux 9 ya Red Hat Enterprise Linux system ho.
- Root ya sudo privileges hon.
- `rsyslog` service install ho.
- Terminal access mojood ho.

---

# Step 1 – rsyslog Service Ka Status Check Karein

Configuration file edit karne se pehle verify karein ke service chal rahi hai.

```bash
systemctl status rsyslog
```

Expected Output:

```text
Active: active (running)
```

Agar service run nahi ho rahi ho to:

```bash
systemctl start rsyslog
```

Boot ke baad automatically start karne ke liye:

```bash
systemctl enable rsyslog
```

---

# Step 2 – rsyslog Configuration File Locate Karein

Main configuration file:

```text
/etc/rsyslog.conf
```

Additional configuration files:

```text
/etc/rsyslog.d/
```

Configuration file ko open karein.

```bash
vim /etc/rsyslog.conf
```

---

# Step 3 – Configuration Rules Ko Samjhein

Configuration file ke andar bohot si rules likhi hoti hain.

Har rule ka format same hota hai.

```text
Facility.Priority      Destination
```

Example:

```text
authpriv.*     /var/log/secure
```

Matlab:

- Facility = Authentication
- Priority = Har severity
- Destination = `/var/log/secure`

---

# Step 4 – Kernel Messages

Configuration file mein aap ko kuch is tarah ki entry mil sakti hai.

```text
#kern.*    /dev/console
```

### Explanation

- `kern` = Kernel Facility
- `*` = Har Priority
- `/dev/console` = Console Device

Ye rule tamam Kernel messages ko console par bhejta hai.

Lekin line ke start mein `#` laga hua hai.

Is ka matlab hai ke ye rule **commented** hai aur active nahi hai.

Agar isay enable karna ho to sirf `#` hata dein.

```text
kern.*    /dev/console
```

---

# Step 5 – General System Messages

Ye sab se common rules mein se ek hai.

```text
*.info;mail.none;authpriv.none;cron.none    /var/log/messages
```

### Explanation

| Part | Matlab |
|------|---------|
| `*` | Har Facility |
| `.info` | Info level aur us se upar |
| `mail.none` | Mail logs ko ignore karo |
| `authpriv.none` | Authentication logs ko ignore karo |
| `cron.none` | Cron logs ko ignore karo |
| Destination | `/var/log/messages` |

Simple alfaaz mein:

Har facility ke Information level aur us se upar ke tamam messages ko `/var/log/messages` mein save karo.

Lekin:

- Mail logs nahi
- Authentication logs nahi
- Cron logs nahi

---

# Step 6 – Authentication Logs

Authentication ke liye commonly ye rule use hota hai.

```text
authpriv.*    /var/log/secure
```

Authentication logs mein shamil hote hain:

- SSH Login
- Failed Password Attempts
- sudo Commands
- su Commands
- PAM Authentication

Ye tamam logs is file mein save hote hain.

```text
/var/log/secure
```

Ye file restricted hoti hai kyun ke is mein sensitive security information hoti hai.

---

# Step 7 – Mail Logs

Mail logs ke liye common rule:

```text
mail.*    /var/log/maillog
```

Matlab:

- Facility = Mail
- Priority = Har Severity
- Destination = `/var/log/maillog`

Mail server ke tamam events is file mein save honge.

---

# Step 8 – Emergency Messages

Kabhi kabhi aap ko ye rule bhi nazar aayega.

```text
*.emerg    *
```

Explanation:

- Pehla `*` = Har Facility
- `emerg` = Emergency Level
- Akhri `*` = Har Logged-in User

Yani agar emergency message aaye to system usay tamam logged-in users ko show karega.

---

# Step 9 – Critical News Messages

Example:

```text
uucp,news.crit    /var/log/spooler
```

Matlab:

- Facility = UUCP aur News
- Priority = Critical aur us se upar
- Destination = `/var/log/spooler`

Sirf Critical messages record honge.

---

# Step 10 – Boot Messages

Boot logs ke liye common rule:

```text
local7.*    /var/log/boot.log
```

Matlab:

- Facility = local7
- Priority = Har Severity
- Destination = `/var/log/boot.log`

Boot ke tamam messages is file mein save honge.

---

# Step 11 – Custom Application Logging

Suppose aap ne Linux server par apni application install ki hai.

Aap chahte hain ke us application ke tamam logs ek alag file mein save hon.

To rsyslog mein aap Facility, Priority aur Destination specify karenge.

Example:

```text
local0.info    /var/log/myapp.log
```

Matlab:

- Facility = local0
- Priority = Information aur us se upar
- Destination = `/var/log/myapp.log`

Ab jo bhi application `local0` facility use karegi us ke logs:

```text
/var/log/myapp.log
```

mein save honge.

---

# Step 12 – Configuration Ke Baad rsyslog Restart Karein

Configuration file change karne ke baad service restart karna zaroori hota hai.

```bash
systemctl restart rsyslog
```

Status verify karein.

```bash
systemctl status rsyslog
```

---

# Step 13 – Log Files Verify Karein

General messages dekhein.

```bash
tail /var/log/messages
```

Authentication logs dekhein.

```bash
tail /var/log/secure
```

Mail logs dekhein.

```bash
tail /var/log/maillog
```

Cron logs dekhein.

```bash
tail /var/log/cron
```

Boot logs dekhein.

```bash
tail /var/log/boot.log
```

---

# 🔬 Practice Exercises

## Exercise 1

Check karein ke rsyslog service chal rahi hai.

```bash
systemctl status rsyslog
```

---

## Exercise 2

Configuration file open karein.

```bash
vim /etc/rsyslog.conf
```

---

## Exercise 3

Authentication logs wali rule dhoondein.

Expected Answer:

```text
authpriv.*    /var/log/secure
```

---

## Exercise 4

Mail logs wali rule dhoondein.

Expected Answer:

```text
mail.*    /var/log/maillog
```

---

## Exercise 5

Boot logs wali rule dhoondein.

Expected Answer:

```text
local7.*    /var/log/boot.log
```

---

## Exercise 6

rsyslog restart karein.

```bash
systemctl restart rsyslog
```

---

## Exercise 7

Last 20 system messages dekhein.

```bash
tail -20 /var/log/messages
```

---

# 📌 Lab Summary

Is lab mein aap ne seekha:

- rsyslog service ko verify karna.
- Configuration file locate karna.
- Rule ki syntax samajhna.

```text
Facility.Priority      Destination
```

- Common logging rules ko samajhna.
- Facilities kis tarah log source ko identify karti hain.
- Priorities kis tarah severity batati hain.
- Mukhtalif log files mein logs ko store karna.
- Configuration ke baad rsyslog restart karna.
- `tail` command ki madad se logs ko verify karna.

---

# 🧠 Yaad Rakhein

> **Rsyslog bilkul Post Office ki tarah kaam karta hai.**
>
> - **Facility** batati hai ke **message kis Department ya Service ne generate kiya hai.**
> - **Priority** batati hai ke **message kitna serious ya urgent hai.**
> - **Destination** batata hai ke **message kis log file mein save hoga.**
>
> ### Yaad Rakhne Ka Formula:
>
> ```text
> Facility.Priority      Destination
> ```
>
> **Example:**
>
> ```text
> authpriv.*      /var/log/secure
> ```
>
> Matlab:
>
> Authentication ke tamam messages `/var/log/secure` mein save karo.