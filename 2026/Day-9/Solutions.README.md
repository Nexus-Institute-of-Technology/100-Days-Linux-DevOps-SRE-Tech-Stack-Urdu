# Module 02 – Linux Filesystem
## Roman Urdu Solution Guide (Day 09)

> Based on Linux Filesystem class notes aur lab discussions.
> May 17th, 2026

---

# Task 1 - Understanding the Linux Filesystem

## Answers

### 1. Filesystem kya hota hai?

Filesystem woh tareeqa hai jiske zariye Linux files aur directories ko organize, store aur manage karta hai.

---

### 2. Root directory `/` kya hoti hai?

Root directory `/` Linux ki sab se upar wali directory hoti hai. Tamam files aur directories isi se start hoti hain.

---

### 3. Linux ko hierarchical filesystem kyun kehte hain?

Linux ko hierarchical filesystem is liye kehte hain kyunke yeh tree structure ki tarah organized hota hai jahan tamam directories root `/` se branch karti hain.

---

### 4. Directories ka purpose kya hota hai?

Directories files ko organize karti hain aur data ko manage karna asaan banati hain.

---

# Task 2 - Explore the Root Filesystem

## Directory Purposes

| Directory | Purpose |
|-----------|----------|
| /boot | Bootloader aur kernel files store karta hai |
| /home | Users ki home directories store karta hai |
| /etc | Configuration files store karta hai |
| /dev | Device files contain karta hai |
| /var | Logs aur variable data store karta hai |
| /tmp | Temporary files store karta hai |
| /usr | Applications aur utilities contain karta hai |
| /proc | Virtual filesystem jo process aur kernel info dikhata hai |

---

# Task 3 - Understanding Linux Users and Prompts

## Answers

### 1. Root aur regular user mein difference?

| Root User | Regular User |
|------------|--------------|
| Full administrative access | Limited permissions |
| System files modify kar sakta hai | Critical settings change nahi kar sakta |
| Symbol `#` hota hai | Symbol `$` hota hai |

---

### 2. sudo kya hota hai?

sudo regular user ko administrative privileges ke saath commands run karne deta hai.

---

### 3. Root user ko kaunsa symbol represent karta hai?

```bash
#
```

---

### 4. Normal user ko kaunsa symbol represent karta hai?

```bash
$
```

---

# Task 4 - Install and Use the tree Command

## Answers

### 1. `tree -d` kya show karta hai?

Sirf directories show karta hai.

---

### 2. `-L 2` ka kya matlab hai?

Yeh directory depth ko 2 levels tak limit karta hai.

---

### 3. tree command useful kyun hai?

Yeh directories ko visual tree structure mein display karta hai.

---

# Task 5 - Linux Filesystem Layers

## Answers

### Logical File System

Applications aur filesystem ke darmiyan interface provide karta hai.

---

### Virtual File System (VFS)

Linux kernel ke andar abstraction layer hoti hai jo multiple filesystem types support karti hai.

---

### Physical File System

Disk par physically stored filesystem hota hai jaise ext4 ya xfs.

---

### 1. VFS kya hota hai?

VFS Linux ko mukhtalif filesystem types ke saath common interface use karne deta hai.

---

### 2. `/proc/cpuinfo` virtual data kyun hai?

Kyunke yeh data dynamically kernel generate karta hai aur disk par physically store nahi hota.

---

# Task 6 - Practice the ls Command

## Answers

### 1. `ls -l /` aur `ls -ld /` mein difference?

| Command | Purpose |
|----------|----------|
| ls -l / | `/` ke andar ki files aur directories list karta hai |
| ls -ld / | `/` directory ki metadata show karta hai |

---

### 2. `-i` kya display karta hai?

Inode numbers display karta hai.

---

### 3. Metadata kya hoti hai?

Metadata file ke bare mein information hoti hai jaise:

- File size
- Permissions
- Owner
- Timestamp
- File type

---

# Task 7 - Understanding Metadata and Inodes

## Answers

### 1. Inode kya information store karta hai?

Inode store karta hai:

- File permissions
- File owner
- File size
- Timestamps
- File type
- Data block locations

---

### 2. Kya inode file ka actual data store karta hai?

Nahin. Inode metadata store karta hai, actual data nahi.

---

### 3. Inodes important kyun hain?

Linux files ko efficiently locate aur manage karne ke liye inodes use karta hai.

---

# Task 8 - Filesystem Snapshot Activity

## Sample Filesystem Tree

```bash
/
├── boot
├── home
├── etc
├── dev
├── usr
├── var
├── tmp
```

---

# Task 9 - Absolute vs Relative Paths

## Answers

### 1. Absolute path kya hota hai?

Absolute path root `/` se start hota hai.

Example:

```bash
/etc/passwd
```

---

### 2. Relative path kya hota hai?

Relative path current working directory se start hota hai.

Example:

```bash
cd dir1
```

---

### 3. Examples

| Type | Example |
|------|----------|
| Absolute Path | /home/user/file.txt |
| Relative Path | ../file.txt |

---

# Task 10 - Linux Boot Process

## Answer

Agar bootloader corrupt ho jaye to Linux boot nahi karega kyunke kernel memory mein load nahi ho payega.

---

# Task 11 - Explore the /boot Directory

## Important Files

| File | Purpose |
|------|----------|
| grub.cfg | GRUB bootloader configuration |
| initramfs | Temporary root filesystem jo boot ke waqt load hota hai |
| vmlinuz | Linux kernel image |
| System.map | Kernel symbol table |

---

# Task 12 - BIOS vs UEFI

## Answers

### BIOS

- Purana firmware system
- MBR partitioning use karta hai
- 2TB disk limit hoti hai

---

### UEFI

- Modern firmware system
- GPT partitioning use karta hai
- Faster aur secure hota hai

---

# Task 13 - POST and Hardware Checks

## POST kya check karta hai?

- CPU
- RAM
- Storage devices
- Keyboard
- GPU
- Dusre hardware devices

---

## Bonus

Agar RAM POST ke dauran fail ho jaye to system boot nahi karega ya beep sounds dega.

---

# Task 14 - Bootloader and Kernel

## Answers

### GRUB

GRUB ka matlab Grand Unified Bootloader hai.

---

### Kernel

Kernel hardware aur software ke darmiyan communication manage karta hai.

---

# Task 15 - Systemd Targets

## Answers

Targets Linux ke operating state ko define karte hain.

Examples:

| Target | Purpose |
|--------|----------|
| multi-user.target | Non-graphical multi-user mode |
| graphical.target | GUI mode |
| rescue.target | Rescue/single-user mode |

---

# Task 16 - Shutdown and Reboot Commands

## Answers

### 1. reboot aur poweroff mein difference?

| Command | Purpose |
|----------|----------|
| reboot | System restart karta hai |
| poweroff | System completely band karta hai |

---

### 2. `shutdown -c` kya karta hai?

Scheduled shutdown cancel karta hai.

---

# Task 17 - Understanding /home

## Answers

### 1. `/home` ka purpose kya hai?

Users ki personal files aur directories store karta hai.

---

### 2. User login ke baad kahan land karta hai?

User apni home directory mein land karta hai.

Example:

```bash
/home/user1
```

---

### 3. Initialization files important kyun hain?

Yeh login ke waqt user environment configure karti hain.

---

# Task 18 - Understanding /dev

## Answers

### 1. Linux mein har cheez ko file kyun treat kiya jata hai?

Linux hardware interaction ko simplify karne ke liye devices ko files treat karta hai.

---

### 2. `/dev/sda` kya hota hai?

Yeh pehli hard disk ko represent karta hai.

---

### 3. udev kya hota hai?

udev dynamically device files manage karta hai.

---

# Task 19 - Block Devices vs Character Devices

## Answers

| Block Devices | Character Devices |
|---------------|------------------|
| Blocks mein access hota hai | Stream ki tarah access hota hai |
| Hard disks | Keyboard |
| SSDs | Mouse |

---

### Hard drives block devices kyun hoti hain?

Kyunke yeh data fixed-size blocks mein read/write karti hain.

---

# Task 20 - TTY and Pseudo Terminals

## Answers

### 1. tty1 kya hota hai?

Machine ke saath directly connected virtual terminal.

---

### 2. pts/0 kya hota hai?

Pseudo-terminal jo aksar SSH sessions mein use hota hai.

---

### 3. `w` command kya display karta hai?

Logged-in users aur unki activities show karta hai.

---

# Task 21 - Disk Commands Practice

## Commands Purpose

| Command | Purpose |
|----------|----------|
| df -hT | Disk usage aur filesystem type show karta hai |
| lsblk | Block devices display karta hai |
| fdisk -l | Disk partitions list karta hai |

---

# Task 22 - Sensors in Linux

## Answers

### 1. lm_sensors kya karta hai?

CPU temperature aur fan speed jaise hardware sensors monitor karta hai.

---

### 2. Data centers mein sensors important kyun hain?

Overheating aur hardware health monitor karne ke liye important hain.

---

# Task 23 - Understanding /etc

## Important Files

| File | Purpose |
|------|----------|
| /etc/passwd | User account information |
| /etc/shadow | Encrypted passwords |
| /etc/group | Group information |
| /etc/sudoers | sudo permissions |

---

## Bonus

`/etc` ko backup karna zaroori hai kyunke ismein critical system configuration files hoti hain.

---

# Task 24 - Understanding /var

## Important Subdirectories

| Directory | Purpose |
|------------|----------|
| /var/log | System logs |
| /var/cache | Cached files |
| /var/tmp | Temporary files |
| /var/spool | Queued jobs |

---

### `/var` bada kyun ho sakta hai?

Kyunke logs, cache aur runtime data waqt ke saath continuously increase hota rehta hai.

---

# Task 25 - Linux Logging System

## Answers

### 1. rsyslog aur journald mein difference?

| rsyslog | journald |
|----------|-----------|
| Traditional logging system | systemd logging system |
| Text logs store karta hai | Binary logs store karta hai |

---

### 2. journalctl kya karta hai?

systemd-journald ke collected logs ko read aur display karta hai.

---

### 3. Logs kahan store hoti hain?

Usually:

```bash
/var/log
```

---

# Task 26 - Understanding logrotate

## Answers

### 1. logrotate kya hota hai?

logrotate automatically old log files ko rotate, compress aur delete karta hai.

---

### 2. Log rotation important kyun hai?

Yeh disk ko full hone se bachata hai.

---

### 3. Agar logs rotate na hon to kya hoga?

Disk full ho sakti hai aur services crash kar sakti hain.

---

# Task 27 - Final Reflection

## Sample Reflection

Module 02 mein maine Linux filesystem, directories, boot process, inodes, logging aur device files ke bare mein seekha. Sabse interesting directory `/dev` thi kyunke Linux devices ko bhi files treat karta hai. Sabse useful command `lsblk` thi kyunke yeh storage devices clearly show karti hai. Inodes ka concept shuru mein mushkil tha. Main filesystem navigation aur Linux logging ki aur practice karna chahta hoon.

---