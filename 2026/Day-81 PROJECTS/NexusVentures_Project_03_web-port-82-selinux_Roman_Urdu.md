# NexusVentures Project 03: TCP Port 82 par Mehfooz Apache Service

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

`/var/www/html` ka web content Apache ke zariye port 82 par accessible banayein, jabke SELinux aur firewalld enabled rahen.

## 2. Karobari Manzar Nama

NexusVentures ko ghair mamooli port par ek andarooni application endpoint chahiye. Apache, SELinux aur firewalld ki configuration ek doosre ke mutabiq honi lazmi hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- `whoami` se account ki tasdeeq karein; mutawaqqa output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
- Change se pehle ka saboot `/root/nexusventures-project03/` ke andar save karein.
Project 02 mein Apache pehle hi install ho jana chahiye.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Zaroori packages install karein

```bash
dnf install -y httpd policycoreutils-python-utils
mkdir -p /root/nexusventures-project03/evidence
```

### Qadam 2: Maujooda content mehfooz rakhein

```bash
find /var/www/html -maxdepth 2 -printf '%M %u:%g %p\n'   > /root/nexusventures-project03/evidence/content-before.txt
```

Instructor ki di hui files delete na karein. Agar directory khali ho to:

```bash
echo 'NexusVentures service on port 82' > /var/www/html/index.html
```

### Qadam 3: Makhsoos Apache drop-in banayein

```bash
SERVER_NAME=$(hostname -f)
cat > /etc/httpd/conf.d/nexusventures-82.conf <<EOF
Listen 82
<VirtualHost *:82>
    ServerName ${SERVER_NAME}
    DocumentRoot /var/www/html
    <Directory /var/www/html>
        Require all granted
    </Directory>
    ErrorLog logs/nexus82_error.log
    CustomLog logs/nexus82_access.log combined
</VirtualHost>
EOF
```

### Qadam 4: Syntax ki tasdeeq karein

```bash
httpd -t
```

Mutawaqqa nateeja: `Syntax OK`.

### Qadam 5: SELinux port type configure karein

```bash
getenforce
semanage port -l | grep '^http_port_t'
```

Agar TCP 82 list mein maujood na ho to:

```bash
semanage port -a -t http_port_t -p tcp 82
```

Agar port 82 pehle se kisi doosre SELinux type ke sath mapped ho to `-m` istemal karne se pehle jaiza lein.

```bash
restorecon -Rv /var/www/html
ls -Zd /var/www/html
```

### Qadam 6: firewalld configure karein

```bash
ZONE=$(firewall-cmd --get-default-zone)
firewall-cmd --permanent --zone="$ZONE" --add-port=82/tcp
firewall-cmd --reload
firewall-cmd --zone="$ZONE" --query-port=82/tcp
```

### Qadam 7: Apache start aur enable karein

```bash
systemctl enable --now httpd
systemctl status httpd --no-pager
ss -ltnp | grep ':82'
```

### Qadam 8: Content test karein

```bash
SERVER_IP=$(hostname -I | awk '{print $1}')
curl --fail http://127.0.0.1:82/
curl --fail "http://${SERVER_IP}:82/"
tail -n 20 /var/log/httpd/nexus82_access.log
```

### Qadam 9: Reboot ke baad tasdeeq

```bash
reboot
```

Phir:

```bash
httpd -t
systemctl is-active httpd
ss -ltn | grep ':82'
firewall-cmd --query-port=82/tcp
semanage port -l | grep '^http_port_t' | grep 82
curl --fail http://127.0.0.1:82/
getenforce
```

## 6. Lazmi Tasdeeq

```bash
httpd -t
systemctl is-enabled httpd
systemctl is-active httpd
firewall-cmd --query-port=82/tcp
semanage port -l | grep '^http_port_t' | grep 82
curl --fail http://127.0.0.1:82/
getenforce | grep Enforcing
```

## 7. Students ko Jama Karne Wale Saboot

Apache drop-in, syntax test, SELinux port mapping, web contexts, firewall rule, listener output, curl ka nateeja, access log aur reboot ke baad ka test jama karein.

## 8. Rollback ya Safai

```bash
systemctl disable --now httpd
rm -f /etc/httpd/conf.d/nexusventures-82.conf
firewall-cmd --permanent --zone="$ZONE" --remove-port=82/tcp
firewall-cmd --reload
```
SELinux port mapping sirf us waqt hatayein jab ise isi project ne banaya ho aur kisi doosri service ko is ki zaroorat na ho.

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
