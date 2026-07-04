# DAY 48  - May 24th, 2026
# Linux Filesystem
---
> Based on Linux Filesystem class notes and lab discussions.
> May 9th, 2026 Class
---

# Task 1 - Understanding the Linux Filesystem

## Answers

### 1. What is a filesystem?

A filesystem is the method Linux uses to organize, store, and manage files and directories on storage devices.

---

### 2. What is the root directory `/`?

The root directory `/` is the top-most directory in Linux. All files and directories begin from `/`.

---

### 3. Why is Linux called a hierarchical filesystem?

Linux is called a hierarchical filesystem because it is organized like a tree structure where all directories branch from the root `/`.

---

### 4. What is the purpose of directories?

Directories organize files and help users manage data efficiently.

---

# Task 2 - Explore the Root Filesystem

## Directory Purposes

| Directory | Purpose |
|-----------|----------|
| /boot | Stores bootloader and kernel files |
| /home | Stores user home directories |
| /etc | Stores configuration files |
| /dev | Contains device files |
| /var | Stores variable data like logs |
| /tmp | Stores temporary files |
| /usr | Contains user applications and utilities |
| /proc | Virtual filesystem containing process/kernel information |

---

# Task 3 - Understanding Linux Users and Prompts

## Answers

### 1. Difference between root and regular user?

| Root User | Regular User |
|------------|--------------|
| Full administrative access | Limited permissions |
| Can modify system files | Cannot change critical system settings |
| Symbol is `#` | Symbol is `$` |

---

### 2. What is sudo?

sudo allows a regular user to execute commands with administrative privileges.

---

### 3. Which symbol represents the root user?

```bash
#
```

---

### 4. Which symbol represents a normal user?

```bash
$
```

---

# Task 4 - Install and Use the tree Command

## Answers

### 1. What does `tree -d` show?

It displays only directories.

---

### 2. What does `-L 2` mean?

It limits the directory depth to 2 levels.

---

### 3. Why is the tree command useful?

It visually displays the directory structure in a hierarchical format.

---

# Task 5 - Linux Filesystem Layers

## Answers

### Logical File System

Provides the interface between applications and filesystems.

---

### Virtual File System (VFS)

An abstraction layer inside the Linux kernel that supports different filesystem types.

---

### Physical File System

The actual filesystem stored on disk such as ext4 or xfs.

---

### 1. What is VFS?

VFS allows Linux to work with multiple filesystem types using a common interface.

---

### 2. Why is `/proc/cpuinfo` considered virtual data?

Because the data is generated dynamically by the kernel and not physically stored on disk.

---

# Task 6 - Practice the ls Command

## Answers

### 1. Difference between `ls -l /` and `ls -ld /`?

| Command | Purpose |
|----------|----------|
| ls -l / | Lists contents inside `/` |
| ls -ld / | Shows metadata of `/` itself |

---

### 2. What does `-i` display?

It displays inode numbers.

---

### 3. What is metadata?

Metadata is information about a file such as:

- File size
- Permissions
- Owner
- Timestamp
- File type

---

# Task 7 - Understanding Metadata and Inodes

## Answers

### 1. What information does an inode store?

An inode stores:

- File permissions
- File owner
- File size
- Timestamps
- File type
- Data block locations

---

### 2. Does an inode store file data?

No. It stores metadata, not actual file contents.

---

### 3. Why are inodes important?

Linux uses inodes to locate and manage files efficiently.

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

### 1. What is an absolute path?

An absolute path starts from the root `/`.

Example:

```bash
/etc/passwd
```

---

### 2. What is a relative path?

A relative path starts from the current working directory.

Example:

```bash
cd dir1
```

---

### 3. Practice examples

| Type | Example |
|------|----------|
| Absolute Path | /home/user/file.txt |
| Relative Path | ../file.txt |

---

# Task 10 - Linux Boot Process

## Answer

If the bootloader is corrupted, Linux may fail to boot because the kernel cannot be loaded into memory.

---

# Task 11 - Explore the /boot Directory

## Important Files

| File | Purpose |
|------|----------|
| grub.cfg | GRUB bootloader configuration |
| initramfs | Temporary root filesystem loaded during boot |
| vmlinuz | Linux kernel image |
| System.map | Kernel symbol table |

---

# Task 12 - BIOS vs UEFI

## Answers

### BIOS

- Older firmware system
- Uses MBR partitioning
- Limited to 2TB disks

---

### UEFI

- Modern firmware system
- Uses GPT partitioning
- Faster and more secure

---

# Task 13 - POST and Hardware Checks

## POST Checks

- CPU
- RAM
- Storage devices
- Keyboard
- GPU
- Other hardware devices

---

## Bonus

If RAM fails during POST, the system may beep or fail to boot.

---

# Task 14 - Bootloader and Kernel

## Answers

### GRUB

GRUB stands for Grand Unified Bootloader.

---

### Kernel

The kernel manages hardware resources and communication between hardware and software.

---

# Task 15 - Systemd Targets

## Answers

Targets define the operating state of Linux.

Examples:

| Target | Purpose |
|--------|----------|
| multi-user.target | Non-graphical multi-user mode |
| graphical.target | GUI mode |
| rescue.target | Rescue/single-user mode |

---

# Task 16 - Shutdown and Reboot Commands

## Answers

### 1. Difference between reboot and poweroff?

| Command | Purpose |
|----------|----------|
| reboot | Restarts the system |
| poweroff | Completely shuts down the system |

---

### 2. What does `shutdown -c` do?

It cancels a scheduled shutdown.

---

# Task 17 - Understanding /home

## Answers

### 1. What is the purpose of `/home`?

It stores user personal files and directories.

---

### 2. Where does a user land after login?

Users land in their home directory.

Example:

```bash
/home/user1
```

---

### 3. Why are initialization files important?

They configure the user environment during login.

---

# Task 18 - Understanding /dev

## Answers

### 1. Why is everything treated as a file in Linux?

Linux treats devices as files to provide a consistent interface for hardware interaction.

---

### 2. What is `/dev/sda`?

It represents the first hard disk.

---

### 3. What is udev?

udev dynamically manages device files in Linux.

---

# Task 19 - Block Devices vs Character Devices

## Answers

| Block Devices | Character Devices |
|---------------|------------------|
| Accessed in blocks | Accessed as streams |
| Hard disks | Keyboard |
| SSDs | Mouse |

---

### Hard drives are block devices because:

They read and write data in fixed-size blocks.

---

# Task 20 - TTY and Pseudo Terminals

## Answers

### 1. What is tty1?

A virtual terminal directly attached to the machine.

---

### 2. What is pts/0?

A pseudo-terminal commonly used in SSH sessions.

---

### 3. What does the `w` command display?

It shows logged-in users and their activities.

---

# Task 21 - Disk Commands Practice

## Commands Purpose

| Command | Purpose |
|----------|----------|
| df -hT | Shows disk usage and filesystem type |
| lsblk | Displays block devices |
| fdisk -l | Lists disk partitions |

---

# Task 22 - Sensors in Linux

## Answers

### 1. What does lm_sensors do?

It monitors hardware sensors such as CPU temperature and fan speed.

---

### 2. Why are sensors important in data centers?

They help monitor overheating and hardware health.

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

`/etc` should always be backed up because it contains critical system configuration files.

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

### Why can `/var` grow large?

Because logs, caches, and runtime data continuously increase over time.

---

# Task 25 - Linux Logging System

## Answers

### 1. Difference between rsyslog and journald?

| rsyslog | journald |
|----------|-----------|
| Traditional logging system | systemd logging system |
| Stores text logs | Stores binary logs |

---

### 2. What does journalctl do?

It reads and displays logs collected by systemd-journald.

---

### 3. Where are logs stored?

Usually in:

```bash
/var/log
```

---

# Task 26 - Understanding logrotate

## Answers

### 1. What is logrotate?

logrotate automatically rotates, compresses, and deletes old log files.

---

### 2. Why is log rotation important?

It prevents logs from filling up disk space.

---

### 3. What happens if logs are never rotated?

The disk may become full and services may crash.

---

# Task 27 - Final Reflection

## Sample Reflection

In Module 02, I learned about the Linux filesystem, directories, boot process, inodes, logging, and device files. The most interesting directory was `/dev` because Linux treats devices as files. The most useful command was `lsblk` because it shows storage devices clearly. Understanding inodes was initially difficult. I would like to practice filesystem navigation and Linux logging more.

---