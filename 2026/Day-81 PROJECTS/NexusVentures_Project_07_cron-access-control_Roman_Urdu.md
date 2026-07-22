# NexusVentures Project 07: Schedule Shuda User Kaam aur Cron Access Control

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`harry` ke liye rozana 12:30 baje `hello` print karne wala cron job banayein aur `natasha` ko cron jobs banane ki permission na dein.

## 2. Karobari Manzar Nama

NexusVentures sirf approved identities ko scheduled processing ki ijazat deta hai aur is baat ka saboot chahta hai ke access control lagu hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Project 04 mein Harry aur Natasha ban chuke hone chahiye.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Users aur service ki tasdeeq karein

```bash
id harry
id natasha
dnf install -y cronie
systemctl enable --now crond
```

### Qadam 2: Maujooda halat ka backup lein

```bash
mkdir -p /root/nexusventures-project07/backups
crontab -u harry -l   > /root/nexusventures-project07/backups/harry-before 2>/dev/null || true
cp -a /etc/cron.deny   /root/nexusventures-project07/backups/cron.deny-before 2>/dev/null || true
```

### Qadam 3: Harry ka job add karein

```bash
{
  crontab -u harry -l 2>/dev/null || true
  echo '30 12 * * * /usr/bin/echo "hello" >> /home/harry/cron-hello.log 2>&1'
} | awk '!seen[$0]++' | crontab -u harry -

crontab -u harry -l
```

Paanch fields minute, hour, mahine ka din, month aur haftay ka din hain.

### Qadam 4: Natasha ko mana karein

```bash
ls -l /etc/cron.allow /etc/cron.deny 2>/dev/null
touch /etc/cron.deny
grep -qxF natasha /etc/cron.deny || echo natasha >> /etc/cron.deny
chmod 600 /etc/cron.deny
```

Agar `/etc/cron.allow` maujood ho to usay tarjeeh milti hai aur instructor ko matlooba policy define karni hogi.

### Qadam 5: Denial test karein

```bash
runuser -l natasha -c 'crontab -l'
runuser -l natasha -c 'crontab -e'
```

Access denial aana mutawaqqa hai.

### Qadam 6: Ikhtiyari tez functional test

```bash
{
  crontab -u harry -l
  echo '* * * * * /usr/bin/date >> /home/harry/cron-minute-test.log 2>&1'
} | awk '!seen[$0]++' | crontab -u harry -

sleep 75
cat /home/harry/cron-minute-test.log
crontab -u harry -l | grep -v 'cron-minute-test.log' | crontab -u harry -
```

### Qadam 7: Logs ka jaiza lein aur reboot karein

```bash
journalctl -u crond -n 30 --no-pager
reboot
```

Phir:

```bash
systemctl is-active crond
crontab -u harry -l
grep -x natasha /etc/cron.deny
```

## 6. Lazmi Tasdeeq

```bash
systemctl is-enabled crond
systemctl is-active crond
crontab -u harry -l | grep '^30 12 '
grep -qxF natasha /etc/cron.deny
! runuser -l natasha -c 'crontab -l'
```

## 7. Students ko Jama Karne Wale Saboot

Harry ka crontab, paanch fields ki wazahat, Natasha ka denial, service status, ikhtiyari minute-test output, journal ka saboot aur reboot validation jama karein.

## 8. Rollback ya Safai

Harry ka save kiya hua crontab bahal karein ya use hata dein:

```bash
if [ -s /root/nexusventures-project07/backups/harry-before ]; then
  crontab -u harry /root/nexusventures-project07/backups/harry-before
else
  crontab -u harry -r
fi
```
`/etc/cron.deny` ko backup se bahal karein ya sirf Natasha wali line hata dein.

## 9. Project Mukammal Karne ki Checklist

- [ ] Sahi VM ki tasdeeq ho gayi
- [ ] Zaroorat par snapshot bana liya gaya
- [ ] Asal halat record kar li gayi
- [ ] Configuration mukammal ho gayi
- [ ] Validation pass ho gayi
- [ ] SELinux ab bhi enforcing hai
- [ ] firewalld ab bhi enabled hai
- [ ] Zaroorat par reboot persistence test kar li gayi
- [ ] Saboot jama kar liye gaye
- [ ] Rollback samajh liya gaya

## 10. Jaiza Sawalat

1. Is project ne kaunsa karobari masla hal kiya?
2. Kis command ne sabit kiya ke configuration active thi?
3. Kis command ne sabit kiya ke configuration mustaqil thi?
4. Kya fail ho sakta hai, aur aap rollback kaise karenge?
