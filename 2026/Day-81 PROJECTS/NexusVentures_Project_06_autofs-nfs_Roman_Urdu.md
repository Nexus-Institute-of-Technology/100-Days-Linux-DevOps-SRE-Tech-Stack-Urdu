# NexusVentures Project 06: AutoFS ke Sath Zaroorat par NFS Access

> **Roman Urdu Class Note:** Commands, file paths, usernames aur configuration values ko bilkul asli soorat mein rakha gaya hai taa-ke lab commands durust chalain.

> **Platform:** Xen Orchestra mein Rocky Linux 9 VM  
> **Account:** `root`  
> **Usool:** SELinux ko enforcing aur firewalld ko enabled rakhein. Mustaqil configuration reboot ke baad bhi kaam karni chahiye.

## 1. Exam Task ko Project mein Tabdeel Kiya Gaya

NFS shares `public` aur `private` ko `/automount` ke neeche khudkar mount karein; public sirf parhne ke liye, private parhne aur likhne ke liye ho, aur 30 seconds tak istemal na hone par dono khudkar unmount ho jayen.

## 2. Karobari Manzar Nama

NexusVentures clients ko markazi documentation aur writable workspace tak zaroorat par access chahiye, baghair is ke ke ghair istemal shuda network mounts hamesha connected rahen.

## 3. Seekhne ke Natayij

Students change ki planning karenge, asal halat record karenge, configuration implement karenge, har command ki wazahat karenge, nateeje ki tasdeeq karenge, jahan zaroori ho reboot ke baad persistence test karenge, aur rollback document karenge.

## 4. Hifazat aur Zaroori Sharaait

- `hostnamectl` aur `ip -brief address` se apni assigned VM ki tasdeeq karein.
- Tasdeeq karein ke `whoami` ka output `root` hai.
- Aisi tabdeeli se pehle jo service ko mutasir kar sakti ho Xen Orchestra snapshot banayein.
Do VMs zaroori hain. Instructor ki taraf se assigned bilkul sahi server IP aur client network istemal karein.

## 5. Qadam-ba-Qadam Hal

## Server VM

### Qadam 1: Assigned values record karein

```bash
NFS_SERVER_IP="192.168.55.151"
CLIENT_NETWORK_CIDR="192.168.55.0/24"
```

Instructor ki taraf se assigned values istemal karein.

### Qadam 2: Install karein aur shares banayein

```bash
dnf install -y nfs-utils
mkdir -p /public /private
echo 'NexusVentures public documentation' > /public/readme.txt
echo 'NexusVentures private workspace' > /private/readme.txt
chmod 0755 /public
chmod 0777 /private
```

`0777` exam-style “tamam users ke liye read-write” task ko poora karta hai. Production mein groups aur zyada sakht permissions istemal honi chahiye.

### Qadam 3: Mehfooz tareeqe se export karein

```bash
mkdir -p /etc/exports.d
cat > /etc/exports.d/nexusventures.exports <<EOF
/public  ${CLIENT_NETWORK_CIDR}(ro,sync,root_squash)
/private ${CLIENT_NETWORK_CIDR}(rw,sync,root_squash)
EOF
exportfs -rav
exportfs -v
```

Sample jawab mein `no_root_squash` istemal hua tha; yeh project zyada mehfooz `root_squash` barqarar rakhta hai.

### Qadam 4: Firewall aur services

```bash
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
systemctl enable --now nfs-server
systemctl is-active nfs-server
showmount -e localhost
```

## Client VM

### Qadam 5: Install karein aur tayyari karein

```bash
dnf install -y nfs-utils autofs
mkdir -p /automount
NFS_SERVER_IP="192.168.55.151"
```

### Qadam 6: Master map banayein

```bash
mkdir -p /etc/auto.master.d
cat > /etc/auto.master.d/nexusventures.autofs <<'EOF'
/automount /etc/auto.nexusventures --timeout=30
EOF
```

### Qadam 7: Indirect map banayein

```bash
cat > /etc/auto.nexusventures <<EOF
public  -fstype=nfs,ro,sync ${NFS_SERVER_IP}:/public
private -fstype=nfs,rw,sync ${NFS_SERVER_IP}:/private
EOF
```

### Qadam 8: Maps ki tasdeeq karein aur start karein

```bash
automount -m
systemctl enable --now autofs
systemctl status autofs --no-pager
```

### Qadam 9: Mounts trigger karein

```bash
findmnt -t nfs,nfs4
ls -l /automount/public
findmnt /automount/public
ls -l /automount/private
findmnt /automount/private
```

### Qadam 10: Access test karein

Public share par write operation fail hona chahiye:

```bash
touch /automount/public/should-fail
```

Private share par write operation kamyab hona chahiye:

```bash
touch /automount/private/client-write-test
ls -l /automount/private/client-write-test
```

### Qadam 11: Timeout test karein

```bash
cd /
sleep 35
findmnt -t nfs,nfs4
```

### Qadam 12: Client reboot karein aur dobara trigger karein

```bash
reboot
```

Reboot ke baad:

```bash
systemctl is-active autofs
ls /automount/public
ls /automount/private
findmnt -t nfs,nfs4
```

## 6. Lazmi Tasdeeq

Server:

```bash
exportfs -v
systemctl is-enabled nfs-server
systemctl is-active nfs-server
firewall-cmd --list-services
```

Client:

```bash
automount -m
systemctl is-enabled autofs
systemctl is-active autofs
findmnt /automount/public
findmnt /automount/private
! touch /automount/public/deny-test
touch /automount/private/write-test
```

## 7. Students ko Jama Karne Wale Saboot

Server exports, firewall services, NFS status, AutoFS master aur map files, mount ka saboot, failed public write, kamyab private write, timeout ka nateeja aur reboot test jama karein.

## 8. Rollback ya Safai

Client:

```bash
systemctl disable --now autofs
rm -f /etc/auto.master.d/nexusventures.autofs /etc/auto.nexusventures
rm -rf /automount
```

Server:

```bash
rm -f /etc/exports.d/nexusventures.exports
exportfs -rav
systemctl disable --now nfs-server
rm -rf /public /private
```

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
