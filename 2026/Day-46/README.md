# RHCSA User Management and Permissions
---
> Date Created: JUNE 22ND, 2026
> Exam Created by: RHCSA Certified Instructor - Muhammed Khalid Khan
---
# RHCSA Objectives Covered
Here are questions that evaluate your capability by RHCSA Exam!
✅ User Creation
✅ User Deletion
✅ Group Creation
✅ Group Deletion
✅ Secondary Groups
✅ Non-Interactive Shells
✅ Password Assignment
✅ Password Aging
✅ Passwordless Sudo
✅ Ownership
✅ Standard Permissions
✅ ACLs
✅ SGID
✅ Sticky Bit
✅ umask
✅ Collaborative Directories
✅ File Access Control
✅ User and Group Verification

---

# Table of Contents

1. User and Group Management
2. Q3 – ACLs and Directory Permissions
3. Q4 – Users, Groups, Shells, Passwords, and Sudo
4. Q5 – SGID Collaborative Directory
5. Q14 – Secondary Group Membership
6. Q17 – ACL Permissions
7. Q18 – Sticky Bit
8. Q19 – SGID Directory
9. Q22 – Create User with UID
10. Q30 – umask
11. Q31 – Password Aging and Sudo
12. Special Permissions Summary
13. Symbolic Mode Examples
14. Common RHCSA Commands

---

# User and Group Management

## Delete a User

### Question

Delete user **jerry** including his home directory and files. If the user is currently logged in, terminate his processes before deleting.

## Task-1
~~~bash
useradd jerry
~~~

### Solution

```bash
userdel -r jerry
```

If logged in:

```bash
pkill -KILL -u jerry
userdel -r jerry
```

Or force deletion:

```bash
userdel -rf jerry
```

---

## Delete a Group

### Question

Delete the group **developers**.

## Task-2
~~~bash
groupadd developers
cat /etc/groups
~~~
### Solution

```bash
groupdel developers
```

---

## Remove User from Group

### Question

Remove a user from a supplementary group.

## Task-3
~~~bash
useradd dev1
groupadd gitadmin
usermod -aG gitadmin dev1
groups dev1
~~~
### Solution

```bash
gpasswd -d username groupname
```

---

# Q3 – ACLs and Directory Permissions

## Question

Create a directory `/share/dev`.

Requirements:

- Ownership should be `devuser:devteam`
- Members of `devteam` should have read, write and execute permissions.
- User `john` should have read-only access using ACL.

---

## Solution

```bash
groupadd devteam
useradd devuser
useradd john

mkdir -p /share/dev

chown devuser:devteam /share/dev

chmod g=rwx /share/dev

setfacl -m u:john:r-- /share/dev

getfacl /share/dev
```

Expected:

```text
user::rwx
user:john:r--
group::rwx
mask::rwx
other::r-x
```

---

# Q4 – Users, Groups, Shells, Passwords, and Sudo

## Question

Create:

- Group `sysadm`
- User `harry` with secondary group `sysadm`
- User `natasha` with secondary group `sysadm`
- User `sarah` with no interactive shell and not a member of sysadm
- Password for all users should be `password`
- Members of sysadm can create users without sudo password
- User harry can reset passwords without being asked for sudo password

---

## Solution

### Create Group

```bash
groupadd sysadm
```

### Create Users

```bash
useradd -G sysadm harry
useradd -G sysadm natasha
```

### Create Non-interactive User

```bash
useradd -s /sbin/nologin sarah
```

### Set Passwords

```bash
echo "password" | passwd --stdin harry
echo "password" | passwd --stdin natasha
echo "password" | passwd --stdin sarah
```

### Configure Sudo

```bash
visudo
```

Add:

```text
%sysadm ALL=(ALL) NOPASSWD: /usr/sbin/useradd

harry ALL=(ALL) NOPASSWD: /usr/bin/passwd [A-Za-z]*,!/usr/bin/passwd root
```

Verify:

```bash
su - harry

sudo passwd natasha
```

---

# Q5 – SGID Collaborative Directory

## Question

Create directory `/shared/sysadm`.

Requirements:

- Group ownership should be sysadm
- Members should have rwx access
- Others should have no access
- Newly created files should automatically inherit group sysadm ownership

---

## Solution

```bash
mkdir -p /shared/sysadm

groupadd sysadm

chgrp sysadm /shared/sysadm

chmod 2770 /shared/sysadm
```

Result:

```text
drwxrws---
```

Meaning:

```text
2 = SGID
770 = rwxrwx---
```

---

# Q14 – Secondary Group Membership

## Question

Create user `alex` and make him a secondary member of group `IT-Team`.

---

## Solution

```bash
groupadd IT-Team

useradd -G IT-Team alex
```

Verify:

```bash
id alex
getent group IT-Team
getent passwd alex
```

Expected:

```text
uid=1013(alex)
gid=1017(alex)
groups=1017(alex),1016(IT-Team)
```

---

# Q17 – ACL Permissions

## Question

Copy `/etc/fstab` to `/var/tmp/fstab`.

Requirements:

- Owner should be root
- Group should be root
- No one should execute it
- Natasha should have read and write access
- Harry should have no permissions
- Everyone else should have read access

---

## Solution

```bash
cp /etc/fstab /var/tmp/
```

Set ACLs:

```bash
setfacl -m u:natasha:rw- /var/tmp/fstab

setfacl -m u:harry:--- /var/tmp/fstab
```

Verify:

```bash
getfacl /var/tmp/fstab
```

Expected:

```text
user::rw-
user:natasha:rw-
user:harry:---
group::r--
mask::rw-
other::r--
```

---

# Q18 – Sticky Bit

## Question

Create directory `/shared/projects`.

Requirements:

- Ownership belongs to alex
- Users should not be able to delete files belonging to other users

---

## Solution

```bash
mkdir -p /shared/projects

chown -R alex /shared/projects

chmod 1777 /shared/projects
```

Result:

```text
drwxrwxrwt
```

Meaning:

```text
1 = Sticky Bit
777 = rwxrwxrwx
```

---

# Q19 – SGID Directory

## Question

Create `/root/d1`.

Set SGID permission on the directory so that all newly created files inherit the group ownership.

---

## Solution

```bash
mkdir -p /root/d1

chmod 2775 /root/d1
```

Result:

```text
drwxrwsr-x
```

Meaning:

```text
2 = SGID
775 = rwxrwxr-x
```

---

# Q22 – Create User with Specific UID

## Question

Create user `unilao`:

- UID should be 2334
- Password should be `ablerate`

---

## Solution

```bash
useradd -u 2334 unilao

echo "ablerate" | passwd --stdin unilao
```

Alternative:

```bash
echo "unilao:ablerate" | chpasswd
```

Complete syntax:

```bash
useradd \
-u 2334 \
-d /home/unilao \
-s /bin/bash \
-g developers \
unilao
```

---

# Q30 – umask

## Question

Configure user `natasha` so that:

Files are created as:

```text
-r--------
```

Directories are created as:

```text
dr-x------
```

---

## Solution

Edit:

```bash
vi /home/natasha/.bashrc
```

Add:

```bash
umask 0266
```

Reload:

```bash
source ~/.bashrc
```

Test:

```bash
touch testfile

mkdir testdir

ls -l
```

Expected:

```text
-r--------
dr-x------
```

---

# Q31(a) Password Aging

## Question

Configure the system so that passwords for newly created users expire after 20 days.

---

## Solution

Edit:

```bash
vi /etc/login.defs
```

Set:

```text
PASS_MAX_DAYS 20
```

Verify:

```bash
chage -l username
```

---

# Q31(b) Passwordless Sudo

## Question

Members of group `admin` should be able to administer the system without entering a password.

---

## Solution

Edit sudoers:

```bash
visudo
```

Add:

```text
%admin ALL=(ALL) NOPASSWD: ALL
```

---

# Special Permissions Summary

| Permission | Numeric | Symbolic | Example |
|------------|---------|----------|----------|
| SUID | 4755 | u+s | -rwsr-xr-x |
| SGID | 2775 | g+s | drwxrwsr-x |
| Sticky Bit | 1777 | +t | drwxrwxrwt |

---

# Symbolic Mode Examples

## SUID

```bash
chmod u+s file
```

or

```bash
chmod 4755 file
```

Result:

```text
-rwsr-xr-x
```

---

## SGID

```bash
chmod g+s directory
```

or

```bash
chmod 2775 directory
```

Result:

```text
drwxrwsr-x
```

---

## Sticky Bit

```bash
chmod +t directory
```

or

```bash
chmod 1777 directory
```

Result:

```text
drwxrwxrwt
```

---

# Common RHCSA Commands

## User Management

```bash
useradd
usermod
userdel
passwd
chpasswd
chage
id
groups
whoami
```

## Group Management

```bash
groupadd
groupmod
groupdel
gpasswd
newgrp
getent group
```

## Ownership

```bash
chown
chgrp
```

## Permissions

```bash
chmod
umask
```

## ACL

```bash
setfacl
getfacl
```

## Sudo

```bash
visudo
sudo
sudo -l
```

## Information Commands

```bash
id
getent passwd
getent group
ls -l
ls -ld
```

---

# RHCSA Objectives Covered

✅ User Creation

✅ User Deletion

✅ Group Creation

✅ Group Deletion

✅ Secondary Groups

✅ Non-interactive Shells

✅ Password Assignment

✅ Password Aging

✅ Passwordless Sudo

✅ Ownership

✅ Standard Permissions

✅ ACLs

✅ SGID

✅ Sticky Bit

✅ umask

✅ Collaborative Directories

✅ User and Group Verification
