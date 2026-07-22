# NexusVentures Project 01: Network ki Bunyad aur Mustaqil Shanakht

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

Hostname, static IPv4 address, gateway aur DNS server configure karein. Sample paper mein `system1.eight.example.com`, `192.168.55.150/24`, gateway `192.168.55.1` aur DNS `8.8.8.8` istemal hua hai.

## 2. Karobari Manzar Nama

NexusVentures ek Linux server ko service ke liye tayyar kar raha hai. Repositories aur services deploy karne se pehle mustahkam network shanakht zaroori hai. Kyun ke tamam students ek hi Xen Orchestra network share karte hain, har VM ko **instructor ki taraf se assigned munfarid IP address** milna lazmi hai.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- `whoami` se account ki tasdeeq karein; mutawaqqa output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
- Change se pehle ka saboot `/root/nexusventures-project01/` ke andar save karein.
- Sirf instructor ki taraf se diya gaya munfarid IP address istemal karein.
- Console access ke baghair activation ka qadam kabhi na karein.

## 5. Qadam-ba-Qadam Hal

### Qadam 1: Xen Orchestra console se kaam karein

Network ki tabdeeli SSH ko disconnect kar sakti hai. Console khula rakhein.

### Qadam 2: Interface aur connection ki pehchan karein

```bash
mkdir -p /root/nexusventures-project01/evidence
nmcli device status
nmcli connection show
ip -brief address
ip route

IFACE=$(ip route show default | awk '{print $5; exit}')
CONNECTION=$(nmcli -g GENERAL.CONNECTION device show "$IFACE")
printf 'Interface=%s\nConnection=%s\n' "$IFACE" "$CONNECTION"
```

Agar dono mein se koi variable khali ho to aage na barhein.

### Qadam 3: Asal halat record karein

```bash
hostnamectl > /root/nexusventures-project01/evidence/hostname-before.txt
ip -brief address > /root/nexusventures-project01/evidence/address-before.txt
ip route > /root/nexusventures-project01/evidence/routes-before.txt
nmcli connection show "$CONNECTION"   > /root/nexusventures-project01/evidence/connection-before.txt
cat /etc/resolv.conf   > /root/nexusventures-project01/evidence/resolv-before.txt
```

### Qadam 4: Instructor ki assigned values dakhil karein

```bash
NEW_HOSTNAME="system1.eight.example.com"
NEW_IP_CIDR="192.168.55.150/24"
NEW_GATEWAY="192.168.55.1"
NEW_DNS="8.8.8.8"
NEW_IP="${NEW_IP_CIDR%/*}"
```

Upar di gayi values sirf misaal hain. Kai students ko kabhi ek hi IP address na dein.

### Qadam 5: Duplicate address check karein

```bash
dnf install -y iputils
arping -D -I "$IFACE" "$NEW_IP" -c 3
```

Agar koi doosra host jawab de to ruk jayein.

### Qadam 6: Hostname set karein

```bash
hostnamectl set-hostname "$NEW_HOSTNAME"
hostnamectl
```

### Qadam 7: Static profile save karein

```bash
nmcli connection modify "$CONNECTION"   ipv4.method manual   ipv4.addresses "$NEW_IP_CIDR"   ipv4.gateway "$NEW_GATEWAY"   ipv4.dns "$NEW_DNS"   ipv4.never-default no

nmcli -f connection.id,connection.interface-name,ipv4.method,ipv4.addresses,ipv4.gateway,ipv4.dns   connection show "$CONNECTION"
```

### Qadam 8: Console se activate karein

```bash
nmcli connection up "$CONNECTION"
```

### Qadam 9: Mukhtalif satahon par tasdeeq karein

```bash
hostnamectl --static
ip -4 address show "$IFACE"
ip route
nmcli device show "$IFACE" | grep -E 'IP4.ADDRESS|IP4.GATEWAY|IP4.DNS'
cat /etc/resolv.conf
ping -c 3 "$NEW_GATEWAY"
ping -c 3 8.8.8.8
getent hosts example.com
curl -I --max-time 10 https://example.com
```

### Qadam 10: Reboot karein aur dobara test karein

```bash
reboot
```

Reboot ke baad:

```bash
hostnamectl --static
ip -4 address show "$IFACE"
ip route
nmcli device show "$IFACE" | grep -E 'IP4.ADDRESS|IP4.GATEWAY|IP4.DNS'
getent hosts example.com
systemctl is-active NetworkManager
getenforce
systemctl is-active firewalld
```

## 6. Lazmi Tasdeeq

In tamam checks ka pass hona lazmi hai:

```bash
test "$(hostnamectl --static)" = "$NEW_HOSTNAME"
ip -4 address show "$IFACE" | grep -F "$NEW_IP_CIDR"
ip route | grep -F "default via $NEW_GATEWAY"
nmcli -g IP4.DNS device show "$IFACE" | grep -F "$NEW_DNS"
getent hosts example.com
```

## 7. Students ko Jama Karne Wale Saboot

Hostname aur network ke pehle aur baad ke saboot, interface aur connection ke naam, duplicate-IP test, gateway test, DNS test aur reboot ke baad ka output jama karein. Wazahat karein ke duplicate IPs mushtarka lab ko kyun mutasir karte hain.

## 8. Rollback ya Safai

`connection-before.txt` mein record ki gayi bilkul asal values bahal karein. Agar asal profile DHCP thi to:

```bash
nmcli connection modify "$CONNECTION"   ipv4.method auto ipv4.addresses "" ipv4.gateway "" ipv4.dns ""
nmcli connection up "$CONNECTION"
hostnamectl set-hostname OLD_HOSTNAME
```

Agar network access bahal na ho sake to Xen Orchestra snapshot istemal karein.

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
