# NIT Academy Linux Practice Lab  
## Module 02 – Linux Filesystem, Paths, Inodes, Devices, Boot Files, Logs  
### Rocky Linux 9 Student Hands-On Lab

> **Goal:** Students will run real commands on their Rocky Linux 9 VM and understand *why* the output appears the way it does.  
> This lab mixes beginner-friendly practice with RHCSA-style tasks.

---

## Lab Rules

1. Log in to your Rocky Linux 9 VM.
2. Run commands exactly as shown.
3. Read the explanation after every command.
4. Do not just copy/paste. Type the commands when possible.
5. Commands beginning with `#` mean run as **root**.
6. Commands beginning with `$` mean run as a **normal user**.

---

## Before You Start

### Check who you are

```bash
whoami
id
hostname
pwd
```

### Expected understanding

| Command | What it shows |
|---|---|
| `whoami` | Current username |
| `id` | UID, GID, and groups |
| `hostname` | System hostname |
| `pwd` | Current working directory |

### Concept

Linux users may be:

| User Type | Prompt | Meaning |
|---|---|---|
| root user | `#` | Full administrative access |
| regular user | `$` | Limited permissions |
| sudo user | `$` | Regular user who can run admin commands using `sudo` |

---

# Part 1 – Understand the Linux Filesystem Tree

## Task 1.1 – Go to the root directory

```bash
cd /
pwd
ls
```

### Expected output idea

You should see directories such as:

```text
bin  boot  dev  etc  home  proc  root  run  tmp  usr  var
```

### Explanation

The root directory `/` is the top of the Linux filesystem.  
Everything starts from `/`.

---

## Task 1.2 – View root directories in long format

```bash
ls -l /
```

### Look carefully at the first character

Example:

```text
drwxr-xr-x.  3 root root   18 May 10 10:00 home
lrwxrwxrwx.  1 root root    7 May 10 10:00 bin -> usr/bin
```

### Meaning

| First Character | Meaning |
|---|---|
| `d` | Directory |
| `-` | Regular file |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |

---

## Task 1.3 – RHCSA-style check

Run this command:

```bash
ls -ld /
```

Now run:

```bash
ls -l /
```

### Question

What is the difference?

### Answer

| Command | Meaning |
|---|---|
| `ls -ld /` | Shows metadata of `/` itself |
| `ls -l /` | Lists contents inside `/` |

---

# Part 2 – Important Linux Directories

## Task 2.1 – Visit important directories

Run:

```bash
cd /
ls -ld /boot /home /etc /dev /var /tmp /usr /proc
```

### Fill this table

| Directory | Purpose |
|---|---|
| `/boot` | Bootloader and kernel files |
| `/home` | User home directories |
| `/etc` | Configuration files |
| `/dev` | Device files |
| `/var` | Variable data like logs and cache |
| `/tmp` | Temporary files |
| `/usr` | User programs and utilities |
| `/proc` | Virtual process and kernel information |

---

## Task 2.2 – Explore `/etc`

```bash
ls /etc | head
ls -l /etc/passwd
ls -l /etc/shadow
ls -l /etc/group
```

### Explanation

`/etc` stores system configuration files.

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted password information |
| `/etc/group` | Group information |

### RHCSA-style question

Which file should be protected more carefully?

```text
/etc/shadow
```

Because it contains password hash information.

---

## Task 2.3 – Explore `/var`

```bash
ls /var
ls /var/log
du -sh /var/log
```

### Explanation

`/var` contains data that changes often.

Examples:

| Directory | Purpose |
|---|---|
| `/var/log` | Logs |
| `/var/cache` | Cached files |
| `/var/tmp` | Temporary files that may survive reboot |
| `/var/spool` | Queued jobs |

---

# Part 3 – Absolute Path vs Relative Path

## Key concept

### Absolute path

Starts from `/`.

Example:

```bash
/etc/passwd
/home/student/file.txt
/var/log/messages
```

### Relative path

Starts from your current directory.

Example:

```bash
../file.txt
documents/file.txt
./script.sh
```

---

## Task 3.1 – Practice absolute paths

Run:

```bash
pwd
cat /etc/os-release
ls -l /etc/passwd
ls -ld /var/log
```

### Explanation

These paths start from `/`, so they work no matter where you are.

Now move somewhere else:

```bash
cd /tmp
pwd
cat /etc/os-release
ls -l /etc/passwd
```

### Question

Did the command still work?

### Answer

Yes. Absolute paths do not depend on your current directory.

---

## Task 3.2 – Practice relative paths

Create a practice structure:

```bash
mkdir -p /tmp/pathlab/homework/linux
mkdir -p /tmp/pathlab/notes
touch /tmp/pathlab/homework/linux/file1.txt
touch /tmp/pathlab/notes/note1.txt
```

Go into the lab:

```bash
cd /tmp/pathlab/homework/linux
pwd
```

Now run:

```bash
ls
ls ..
ls ../..
ls ../../notes
ls ../../notes/note1.txt
```

### Explanation

| Symbol | Meaning |
|---|---|
| `.` | Current directory |
| `..` | Parent directory |
| `../..` | Go up two levels |

---

## Task 3.3 – RHCSA-style path challenge

Current directory:

```bash
cd /tmp/pathlab/homework/linux
pwd
```

You are here:

```text
/tmp/pathlab/homework/linux
```

### Challenge 1

Use a relative path to list:

```text
/tmp/pathlab/notes
```

Command:

```bash
ls ../../notes
```

### Challenge 2

Use an absolute path to list the same directory:

```bash
ls /tmp/pathlab/notes
```

### Challenge 3

From `/tmp/pathlab/homework/linux`, create this file using relative path:

```text
/tmp/pathlab/notes/rhcsa.txt
```

Command:

```bash
touch ../../notes/rhcsa.txt
ls ../../notes
```

---

# Part 4 – Install and Use `tree`

## Task 4.1 – Install tree

As root:

```bash
dnf install tree -y
```

If you are not root, use:

```bash
sudo dnf install tree -y
```

---

## Task 4.2 – Display directory tree

```bash
tree /tmp/pathlab
tree -d /tmp/pathlab
tree -a -L 2 /tmp/pathlab
```

### Explanation

| Command | Meaning |
|---|---|
| `tree /tmp/pathlab` | Shows files and directories |
| `tree -d /tmp/pathlab` | Shows directories only |
| `tree -L 2 /tmp/pathlab` | Limits depth to 2 levels |
| `tree -a` | Shows hidden files also |

---

# Part 5 – Metadata and Inodes

## Task 5.1 – Create files and view metadata

```bash
mkdir -p /tmp/inodelab
cd /tmp/inodelab
touch file1.txt file2.txt
ls -l
ls -li
stat file1.txt
```

### Explanation

`ls -l` shows metadata:

| Field | Meaning |
|---|---|
| permissions | Who can read/write/execute |
| owner | User owner |
| group | Group owner |
| size | File size |
| timestamp | Last modification time |
| name | File or directory name |

`ls -li` adds the inode number.

---

## Task 5.2 – Understand inode vs filename

Run:

```bash
echo "Linux is powerful" > file1.txt
ls -li file1.txt
stat file1.txt
cat file1.txt
```

### Concept

A file has:

| Part | Meaning |
|---|---|
| Filename | Human-friendly name |
| Inode | Metadata record |
| Data blocks | Actual file content |

The inode does **not** store the filename and does **not** store the actual file content.

---

## Task 5.3 – RHCSA-style inode check

Create a hard link:

```bash
ln file1.txt hardlink1.txt
ls -li
```

### Question

Do `file1.txt` and `hardlink1.txt` have the same inode number?

### Expected answer

Yes. A hard link is another filename pointing to the same inode.

Now change the file:

```bash
echo "New line added" >> hardlink1.txt
cat file1.txt
cat hardlink1.txt
```

### Explanation

Both names point to the same file data.

---

# Part 6 – `df`, `du`, and Disk View

## Task 6.1 – Filesystem usage

```bash
df -hT
```

### Explanation

| Option | Meaning |
|---|---|
| `-h` | Human-readable sizes |
| `-T` | Shows filesystem type |

---

## Task 6.2 – Directory usage

```bash
du -sh /etc
du -sh /var/log
du -sh /tmp/pathlab
```

### Difference between `df` and `du`

| Command | Meaning |
|---|---|
| `df` | Shows filesystem usage |
| `du` | Shows directory/file space usage |

---

## Task 6.3 – Inode usage

```bash
df -i
```

### Explanation

`df -i` shows inode usage.  
A filesystem can run out of inodes even if disk space is still available.

---

# Part 7 – Virtual Filesystems: `/proc` and `/sys`

## Task 7.1 – View CPU information

```bash
cat /proc/cpuinfo | head
```

### Explanation

`/proc/cpuinfo` looks like a normal file, but it is generated dynamically by the kernel.

It is **virtual data**, not a regular file stored on disk.

---

## Task 7.2 – View memory information

```bash
cat /proc/meminfo | head
free -h
```

### Question

Is `/proc/meminfo` physically stored as a normal text file?

### Answer

No. It is created dynamically by the kernel.

---

# Part 8 – Device Files in `/dev`

## Task 8.1 – Explore devices

```bash
ls /dev | head
ls -l /dev/null
ls -l /dev/zero
lsblk
```

### Explanation

Linux treats devices like files to provide a consistent interface.

| Device | Meaning |
|---|---|
| `/dev/null` | Discards output |
| `/dev/zero` | Produces zero bytes |
| `/dev/sda` or `/dev/xvda` | Disk device |
| `/dev/tty` | Terminal device |

---

## Task 8.2 – Block vs character devices

```bash
ls -l /dev/null
ls -l /dev/zero
ls -l /dev/sda 2>/dev/null || ls -l /dev/xvda 2>/dev/null || ls -l /dev/vda 2>/dev/null
```

### Look at the first character

| First character | Type |
|---|---|
| `b` | Block device |
| `c` | Character device |

### Concept

| Block Device | Character Device |
|---|---|
| Reads/writes in blocks | Reads/writes as stream |
| Hard disk, SSD | Keyboard, mouse, terminal |

---

# Part 9 – Terminal Concepts: TTY and PTS

## Task 9.1 – Check your terminal

```bash
tty
who
w
ls /dev/pts
```

### Explanation

| Term | Meaning |
|---|---|
| `tty1` | Direct virtual terminal |
| `pts/0` | Pseudo-terminal, often SSH or Guacamole session |
| `w` | Shows logged-in users and what they are doing |
| `who` | Shows logged-in users |

---

# Part 10 – Boot Files in `/boot`

## Task 10.1 – Explore `/boot`

```bash
ls -l /boot
find /boot -maxdepth 2 -type f | head
```

Look for files containing:

```text
vmlinuz
initramfs
System.map
grub
```

Try:

```bash
ls -l /boot/vmlinuz*
ls -l /boot/initramfs*
ls -l /boot/System.map*
```

### Explanation

| File | Purpose |
|---|---|
| `vmlinuz` | Linux kernel image |
| `initramfs` | Temporary root filesystem used during boot |
| `System.map` | Kernel symbol table |
| `grub.cfg` | GRUB bootloader configuration |

---

## Task 10.2 – RHCSA-style question

What happens if the bootloader is corrupted?

### Answer

Linux may fail to boot because the kernel cannot be loaded into memory.

---

# Part 11 – Systemd Targets

## Task 11.1 – View current target

```bash
systemctl get-default
systemctl list-units --type=target
```

### Common targets

| Target | Meaning |
|---|---|
| `multi-user.target` | Non-graphical multi-user mode |
| `graphical.target` | GUI mode |
| `rescue.target` | Rescue/single-user mode |

---

## RHCSA-style practice

Do **not** change the target yet. Just view available targets:

```bash
systemctl list-unit-files --type=target
```

---

# Part 12 – Logs and `journalctl`

## Task 12.1 – View system logs

```bash
journalctl -n 20
```

### Explanation

`journalctl` reads logs collected by `systemd-journald`.

---

## Task 12.2 – Follow live logs

In one terminal, run:

```bash
journalctl -f
```

In another terminal, run:

```bash
logger "NIT Academy test log from student"
```

Go back to the first terminal. You should see the message.

Stop following logs with:

```bash
Ctrl + C
```

---

## Task 12.3 – Traditional logs

```bash
ls /var/log
tail -n 20 /var/log/messages 2>/dev/null || echo "/var/log/messages may not exist or may require rsyslog"
```

### Explanation

| Logging system | Meaning |
|---|---|
| `journald` | systemd logging system |
| `rsyslog` | Traditional Linux logging system |

---

# Part 13 – Logrotate

## Task 13.1 – Read logrotate configuration

```bash
cat /etc/logrotate.conf
ls /etc/logrotate.d/
```

### Explanation

`logrotate` rotates, compresses, and removes old logs so disk space does not fill up.

---

## Task 13.2 – RHCSA-style question

What happens if logs are never rotated?

### Answer

The disk can become full and services may crash or stop writing logs.

---

# Part 14 – Shutdown and Reboot Commands

## Read-only learning section

Do **not** reboot during class unless your instructor allows it.

Study these:

```bash
systemctl reboot
shutdown -r now
systemctl poweroff
shutdown -c
```

### Meaning

| Command | Purpose |
|---|---|
| `systemctl reboot` | Reboots the system |
| `shutdown -r now` | Reboots now |
| `systemctl poweroff` | Shuts down completely |
| `shutdown -c` | Cancels scheduled shutdown |

---

# Part 15 – RHCSA-Style Final Challenge

Complete all tasks without looking at the answer first.

---

## Challenge 1 – Create a lab directory

Create:

```text
/tmp/final-lab/linux/filesystem
/tmp/final-lab/linux/logs
/tmp/final-lab/linux/paths
```

### Command

```bash
mkdir -p /tmp/final-lab/linux/{filesystem,logs,paths}
```

Verify:

```bash
tree -d /tmp/final-lab
```

---

## Challenge 2 – Create files

Create:

```text
/tmp/final-lab/linux/filesystem/inode.txt
/tmp/final-lab/linux/logs/messages.txt
/tmp/final-lab/linux/paths/absolute.txt
/tmp/final-lab/linux/paths/relative.txt
```

### Command

```bash
touch /tmp/final-lab/linux/filesystem/inode.txt
touch /tmp/final-lab/linux/logs/messages.txt
touch /tmp/final-lab/linux/paths/absolute.txt
touch /tmp/final-lab/linux/paths/relative.txt
```

---

## Challenge 3 – Use relative paths

Go here:

```bash
cd /tmp/final-lab/linux/filesystem
pwd
```

Create a file in `/tmp/final-lab/linux/paths` using relative path:

```bash
touch ../paths/from-relative-path.txt
```

Verify:

```bash
ls -l ../paths
```

---

## Challenge 4 – Display inode numbers

```bash
ls -li /tmp/final-lab/linux/filesystem
ls -li /tmp/final-lab/linux/paths
```

---

## Challenge 5 – Use `stat`

```bash
stat /tmp/final-lab/linux/filesystem/inode.txt
```

Find:

- inode number
- owner
- group
- permissions
- file size
- timestamps

---

## Challenge 6 – Compare `df` and `du`

```bash
df -hT /tmp
du -sh /tmp/final-lab
```

### Question

Which command shows filesystem usage and which shows directory usage?

---

## Challenge 7 – Use logs

```bash
logger "Final lab completed by $(whoami)"
journalctl -n 10 | grep "Final lab completed" || journalctl -n 20
```

---

# Part 16 – Student Reflection

Write answers in your notebook.

1. What is the difference between `/`, `/root`, and `/home`?
2. Why does an absolute path start with `/`?
3. What does `..` mean in a relative path?
4. What does an inode store?
5. Does an inode store file content?
6. Why is `/proc/cpuinfo` virtual data?
7. What is the difference between `/dev/sda` and `/etc/passwd`?
8. What command shows disk usage?
9. What command shows inode usage?
10. Why is logrotate important?

---

# Instructor Answer Key

## Key answers

| Question | Answer |
|---|---|
| Filesystem | Method Linux uses to organize, store, and manage files/directories |
| Root `/` | Top-most directory |
| Hierarchical filesystem | Tree structure beginning from `/` |
| `/boot` | Bootloader and kernel files |
| `/etc` | Configuration files |
| `/dev` | Device files |
| `/var` | Logs, cache, runtime variable data |
| `/proc` | Virtual process/kernel information |
| `#` prompt | root |
| `$` prompt | regular user |
| `tree -d` | Directories only |
| `-L 2` | Limit depth to 2 levels |
| VFS | Kernel abstraction layer for many filesystem types |
| `/proc/cpuinfo` | Kernel-generated virtual data |
| `ls -l /` | Lists contents inside `/` |
| `ls -ld /` | Shows metadata of `/` itself |
| `ls -i` | Shows inode number |
| inode | Stores metadata and block locations |
| absolute path | Starts from `/` |
| relative path | Starts from current directory |
| `vmlinuz` | Linux kernel image |
| `initramfs` | Temporary boot filesystem |
| `journalctl` | Reads systemd journal logs |
| `logrotate` | Rotates/compresses/removes old logs |

---

# Clean Up

At the end of the lab, students may remove practice files:

```bash
rm -rf /tmp/pathlab /tmp/inodelab /tmp/final-lab
```

---

# End of Lab

Congratulations. You have practiced Linux filesystem navigation, root directories, absolute and relative paths, metadata, inodes, device files, boot files, targets, logs, and logrotate.
