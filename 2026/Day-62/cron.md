# MODULE 07 – Cron Notes (Roman Urdu)
> **08 July 2026 - Budh (Wednesday)**

---

# 1. Cron Kya Hai?

`cron` Linux mein **recurring (baar baar chalne wale)** jobs ko automatically schedule karne ke liye use hota hai.

Cron ki madad se commands ya scripts automatically kisi specific time par chal sakti hain, jaise:

- Har 15 minute baad
- Har roz
- Har Monday dopahar 12:15 baje
- Har mahine

User ke cron jobs ko manage karne ke liye command hai:

```bash
crontab
```

---

# 2. Cron Time aur Date Fields

Har cron entry mein **5 time/date fields** hoti hain aur us ke baad command ya script likhi jati hai.

```text
minute hour day-of-month month day-of-week command
```

| Field | Allowed Values |
|--------|----------------|
| Minute | `0-59` |
| Hour | `0-23` |
| Day of Month | `1-31` |
| Month | `1-12` ya month ka naam |
| Day of Week | `0-7` (0 ya 7 = Sunday) ya day ka naam |

Example:

```cron
*/15 * * * * /script.sh
```

Matlab:

```text
/script.sh har 15 minute baad chalegi.
```

---

# 3. Task 1 – User harry Ka Script Har 15 Minute Chalana

## Requirement

Root user ki taraf se `/script.sh` ko user **harry** ke naam se har 15 minute baad chalana hai.

Script yeh command run karegi:

```bash
echo "Harry's job is executed at $(date)" > /harry_cron
```

---

## Step 1 – Script Banayein

```bash
vim /script.sh
```

Is line ko add karein:

```bash
echo "Harry's job is executed at $(date)" > /harry_cron
```

Save aur exit karein:

```vim
:wq
```

---

## Step 2 – Execute Permission Dein

```bash
chmod +x /script.sh
```

Is se script executable ban jayegi.

---

## Step 3 – Output File Banayein aur Ownership Badlein

```bash
touch /harry_cron
chown harry:harry /harry_cron
```

Is se file create hogi aur us ka owner **harry** ban jayega.

---

## Step 4 – Root Se harry Ka Crontab Edit Karein

```bash
crontab -u harry -e
```

Yeh entry add karein:

```cron
*/15 * * * * /script.sh
```

Save aur exit:

```vim
:wq
```

---

## Step 5 – Verify Karein

```bash
crontab -u harry -l
```

Expected output:

```cron
*/15 * * * * /script.sh
```

---

# 4. Cron Access Control

Root user decide kar sakta hai ke kaun cron use kar sakta hai aur kaun nahi.

Is ke liye do files hoti hain:

```bash
/etc/cron.allow
/etc/cron.deny
```

---

## Cron Access Rules

| Condition | Result |
|-----------|--------|
| `/etc/cron.allow` mojood ho | Sirf is file mein listed users cron use kar sakte hain |
| `/etc/cron.deny` mojood ho | Jo users is file mein listed hon woh cron use nahi kar sakte |
| Dono files mojood hon | `/etc/cron.allow` ki priority zyada hoti hai |
| Dono files na hon | Sirf root cron use kar sakta hai |

### Important Point

```text
/etc/cron.allow ki priority hamesha /etc/cron.deny se zyada hoti hai.
```

---

# 5. Task 2 – Sirf User lara Ko Cron Use Karne Dena

## Requirement

User **lara** ke naam se `/access.sh` script chalani hai.

Yeh script **har Monday dopahar 12:15 PM** par chalegi.

Sirf **lara** ko cron use karne ki permission hogi.

Script:

```bash
echo "Lara's job is executed at $(date)" > /home/lara/cron_lara
```

---

## Step 1 – Sirf lara Ko Allow Karein

```bash
vim /etc/cron.allow
```

Is file mein likhein:

```text
lara
```

Save aur exit:

```vim
:wq
```

Ab sirf **lara** cron use kar sakegi.

---

## Step 2 – Script Banayein

```bash
vim /access.sh
```

Is line ko add karein:

```bash
echo "Lara's job is executed at $(date)" > /home/lara/cron_lara
```

Save aur exit.

---

## Step 3 – lara Ko Script Ki Permission Dein

```bash
setfacl -m u:lara:rx /access.sh
```

Is command se **lara** ko read aur execute permission mil jati hai.

---

## Step 4 – User lara Ban Jayein

```bash
su - lara
```

---

## Step 5 – Current Cron Jobs Dekhein

```bash
crontab -l
```

Agar koi cron job nahi hogi to blank ya "no crontab" aa sakta hai.

---

## Step 6 – Crontab Edit Karein

```bash
crontab -e
```

Yeh entry add karein:

```cron
15 12 * * 1 /access.sh
```

Matlab:

```text
Har Monday dopahar 12:15 baje /access.sh chalegi.
```

Save aur exit.

---

## Step 7 – Verify Karein

```bash
crontab -l
```

Expected output:

```cron
15 12 * * 1 /access.sh
```

---

# 6. Slides Se Important Cron Commands

| Command | Kaam |
|----------|------|
| `crontab -e` | Current user ka crontab edit kare |
| `crontab -l` | Current user ka crontab dekhe |
| `crontab -u harry -e` | Root se harry ka crontab edit kare |
| `crontab -u harry -l` | Root se harry ka crontab dekhe |
| `vim /etc/cron.allow` | Allowed users ki file edit kare |
| `vim /etc/cron.deny` | Denied users ki file edit kare |

---

# 7. Cron Examples

| Cron Entry | Matlab |
|------------|--------|
| `*/15 * * * * /script.sh` | Har 15 minute baad |
| `15 12 * * 1 /access.sh` | Har Monday 12:15 PM |
| `0 0 * * * /script.sh` | Har roz raat 12 baje |
| `0 12 * * * /script.sh` | Har roz dopahar 12 baje |
| `0 8 * * 1-5 /script.sh` | Monday se Friday subah 8 baje |

---

# 8. Student Practice

## Practice 1 – Script Banayein

```bash
vim /script.sh
```

Is line ko add karein:

```bash
echo "Cron job executed at $(date)" > /tmp/cron_test
```

Phir execute permission dein:

```bash
chmod +x /script.sh
```

---

## Practice 2 – Har 15 Minute Schedule Karein

```bash
crontab -e
```

Entry add karein:

```cron
*/15 * * * * /script.sh
```

Verify:

```bash
crontab -l
```

---

## Practice 3 – Cron Entry Samjhein

Cron Entry:

```cron
15 12 * * 1 /access.sh
```

Jawab:

```text
/access.sh har Monday dopahar 12:15 PM par chalegi.
```

---

# 9. Exam Revision Summary

| Topic | Yaad Rakhna Hai |
|--------|-----------------|
| Main command | `crontab` |
| Current user ka cron edit | `crontab -e` |
| Current user ka cron list | `crontab -l` |
| Root se kisi aur user ka cron edit | `crontab -u USER -e` |
| Root se kisi aur user ka cron list | `crontab -u USER -l` |
| Allow file | `/etc/cron.allow` |
| Deny file | `/etc/cron.deny` |
| Har 15 minute | `*/15 * * * *` |
| Monday 12:15 PM | `15 12 * * 1` |

---

# 💡 Yaad Rakhein

> **Cron Linux ka automatic scheduler hai. Agar aap ko koi command ya script kisi specific time ya baar baar chalani ho, to Cron sab se behtareen tool hai.**