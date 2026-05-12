# Welcome to NIT Academy - Day 3

---

# Table of Contents

| Task | Title | Summary |
|------|------|------|
| 1 | [PowerShell ko Administrator Mode mein kholna](#task-1---powershell-ko-administrator-mode-mein-kholna) | Elevated PowerShell kholna |
| 2 | [Check karein kya WSL install hai](#task-2---check-karein-kya-wsl-install-hai) | Maujooda WSL installation verify karna |
| 3 | [Installed Linux Distros check karna](#task-3---installed-linux-distros-check-karna) | Installed Linux distributions dekhna |
| 4 | [Available Distros check karna](#task-4---available-linux-distros-check-karna) | Download hone wali distros dekhna |
| 5 | [WSL Install karna](#task-5---wsl-install-karna) | WSL aur Ubuntu install karna |
| 6 | [Specific Distro install karna](#task-6---specific-distro-install-karna) | Ubuntu, Debian waghera install karna |
| 7 | [Linux ka pehla setup](#task-7---linux-ka-pehla-setup) | Linux username/password banana |
| 8 | [WSL manually start karna](#task-8---wsl-manually-start-karna) | Linux manually launch karna |
| 9 | [Linux verify karna](#task-9---linux-verify-karna) | Linux functionality test karna |
| 10 | [WSL version check karna](#task-10---wsl-version-check-karna) | WSL2 verify karna |
| 11 | [Existing distro ko WSL2 mein convert karna](#task-11---existing-distro-ko-wsl2-mein-convert-karna) | Distro upgrade karna |
| 12 | [Default distro set karna](#task-12---default-distro-set-karna) | Default Linux distro configure karna |
| 13 | [WSL shutdown aur restart karna](#task-13---wsl-shutdown-aur-restart-karna) | Linux environment restart karna |
| 14 | [Windows files ko WSL se access karna](#task-14---windows-files-ko-wsl-se-access-karna) | Windows drives access karna |
| 15 | [Linux files ko Windows se access karna](#task-15---linux-files-ko-windows-se-access-karna) | Linux files Explorer se access karna |
| 16 | [Common commands](#task-16---common-commands-for-students) | Frequently used commands |
| 17 | [Common problems aur fixes](#task-17---common-problems-and-fixes) | Beginner troubleshooting |
| 18 | [Final verification checklist](#task-18---final-student-verification-checklist) | Installation confirm karna |

---

# WSL Kya Hai?

WSL (Windows Subsystem for Linux) Windows ka ek feature hai jo aap ko Windows ke andar directly Linux environment chalane deta hai bina:

- Dual boot
- Alag virtual machine
- Additional hypervisors

ke.

Is se students Windows ke andar directly Linux commands aur development tools use kar sakte hain.

---

# Humein WSL Ki Zarurat Kyun Hai?

Hamari koshish hai ke students ko Open-Source Linux environment diya jaye bina kisi paid software ya subscription ke.

WSL students ko allow karta hai:

- Linux commands practice karne ke liye
- Bash shell use karne ke liye
- Linux tools chalane ke liye
- DevOps/Linux administration seekhne ke liye
- Windows se directly Linux access karne ke liye

---

# Is Guide Mein Kya Kya Hoga

1. WSL install karna
2. Ubuntu Linux install karna
3. Existing WSL installation check karna
4. Linux verify karna
5. Common beginner problems fix karna

---

# Official Microsoft References

| Command | Purpose |
|------|------|
| `wsl --install` | WSL install karna |
| `wsl --list --verbose` | Installed distros dekhna |
| `wsl --list --online` | Downloadable distros dekhna |

Source: Microsoft Learn

---

# Task 1 - PowerShell ko Administrator Mode mein kholna

1. **Start Menu** open karein
2. Type karein:

```powershell
PowerShell
```

3. **Windows PowerShell** par right-click karein
4. Select karein:

```text
Run as administrator
```

---

# Task 2 - Check karein kya WSL install hai

Run karein:

```powershell
wsl --status
```

Agar WSL install hai to aap ko ye information nazar aa sakti hai:

- Default Distribution
- Default Version
- Kernel Version
- WSL Version

---

# Task 3 - Installed Linux Distros check karna

Run karein:

```powershell
wsl --list --verbose
```

Short version:

```powershell
wsl -l -v
```

Example output:

```text
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
```

## Explanation

| Column | Meaning |
|------|------|
| NAME | Installed Linux distro |
| STATE | Running ya stopped |
| VERSION | WSL version |
| `*` | Default distro |

Agar Ubuntu nazar aa raha hai to WSL pehle se install hai.

---

# Task 4 - Available Linux Distros check karna

Run karein:

```powershell
wsl --list --online
```

Short version:

```powershell
wsl -l -o
```

Example:

```text
NAME            FRIENDLY NAME
Ubuntu          Ubuntu
Debian          Debian GNU/Linux
kali-linux      Kali Linux Rolling
openSUSE        openSUSE Leap
```

---

# Task 5 - WSL Install Karna

Run karein:

```powershell
wsl --install
```

Ye install karega:

- WSL
- Ubuntu
- WSL 2

Agar Windows restart maange to computer restart karein.

---

# Task 6 - Specific Distro install karna

Ubuntu install karne ke liye:

```powershell
wsl --install -d Ubuntu
```

Debian install karne ke liye:

```powershell
wsl --install -d Debian
```

Available distro names dekhne ke liye:

```powershell
wsl --list --online
```

---

# Task 7 - Linux ka Pehla Setup

Installation ke baad Linux aapse puchega:

```text
Enter new UNIX username:
New password:
Retype new password:
```

Example username:

```text
student
```

## Important Notes

- Linux username ko Windows username jaisa hona zaruri nahi
- Password type karte waqt kuch nazar nahi aayega
- Password bhoolna nahi

---

# Task 8 - WSL Manually Start Karna

Default distro start karne ke liye:

```powershell
wsl
```

Ubuntu directly start karne ke liye:

```powershell
wsl -d Ubuntu
```

---

# Task 9 - Linux Verify Karna

Linux ke andar ye commands run karein:

```bash
whoami
pwd
uname -a
cat /etc/os-release
```

Expected output example:

```text
NAME="Ubuntu"
VERSION="24.04 LTS"
```

---

# Linux Packages Update Karna

Run karein:

```bash
sudo apt update
```

Phir:

```bash
sudo apt upgrade -y
```

---

# Task 10 - WSL Version Check Karna

Run karein:

```powershell
wsl --version
```

Distro version verify karne ke liye:

```powershell
wsl -l -v
```

Expected:

```text
VERSION
2
```

Example:

```text
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
```

---

# WSL 2 Ko Default Banana

Run karein:

```powershell
wsl --set-default-version 2
```

---

# Task 11 - Existing Distro Ko WSL2 Mein Convert Karna

Agar distro VERSION 1 dikha raha hai:

```powershell
wsl --set-version Ubuntu 2
```

`Ubuntu` ko apne distro name se replace karein.

Phir dobara verify karein:

```powershell
wsl -l -v
```

---

# Task 12 - Default Distro Set Karna

Agar multiple distros installed hain:

```powershell
wsl --set-default Ubuntu
```

Ab:

```powershell
wsl
```

automatically Ubuntu open karega.

---

# Task 13 - WSL Shutdown aur Restart Karna

Shutdown karne ke liye:

```powershell
wsl --shutdown
```

Ubuntu restart karne ke liye:

```powershell
wsl -d Ubuntu
```

---

# Task 14 - Windows Files Ko WSL Se Access Karna

Windows drives `/mnt` ke andar mounted hoti hain.

Example:

```bash
cd /mnt/c/Users
```

Files list karne ke liye:

```bash
ls
```

---

# Task 15 - Linux Files Ko Windows Se Access Karna

File Explorer open karein aur type karein:

```text
\\wsl$
```

Example:

```text
\\wsl$\Ubuntu
```

---

# Task 16 - Common Commands for Students

| Task | Command |
|------|------|
| WSL status check karna | `wsl --status` |
| Installed distros dekhna | `wsl -l -v` |
| Online distros dekhna | `wsl -l -o` |
| WSL install karna | `wsl --install` |
| Ubuntu install karna | `wsl --install -d Ubuntu` |
| WSL start karna | `wsl` |
| Ubuntu start karna | `wsl -d Ubuntu` |
| WSL shutdown karna | `wsl --shutdown` |
| WSL2 default set karna | `wsl --set-default-version 2` |
| Default distro set karna | `wsl --set-default Ubuntu` |

---

# Task 17 - Common Problems aur Fixes

## Problem 1 - `wsl` Command Recognize Nahi Ho Raha

Try karein:

1. Computer restart karein
2. PowerShell Administrator Mode mein kholein
3. Run karein:

```powershell
wsl --install
```

---

## Problem 2 - Linux Distro Installed Nahi Hai

Installed distros check karein:

```powershell
wsl -l -v
```

Downloadable distros dekhne ke liye:

```powershell
wsl -l -o
```

Ubuntu install karein:

```powershell
wsl --install -d Ubuntu
```

---

## Problem 3 - Ubuntu Open Hoke Band Ho Jata Hai

Run karein:

```powershell
wsl -d Ubuntu
```

Agar koi error aaye to usko copy karke instructor ko bhejein.

---

## Problem 4 - Linux Password Bhool Gaye

Root shell open karein:

```powershell
wsl -u root
```

Password reset karein:

```bash
passwd username
```

Example:

```bash
passwd student
```

Exit:

```bash
exit
```

---

## Problem 5 - Distro WSL1 Dikha Raha Hai

Check karein:

```powershell
wsl -l -v
```

Convert karein:

```powershell
wsl --set-version Ubuntu 2
```

---

# Task 18 - Final Student Verification Checklist

Run karein:

```powershell
wsl --status
```

```powershell
wsl -l -v
```

Ubuntu start karein:

```powershell
wsl -d Ubuntu
```

Linux ke andar:

```bash
whoami
pwd
cat /etc/os-release
sudo apt update
```

Agar saari commands successful chalti hain to WSL sahi tarah install ho chuka hai.

---

# Final Summary

Recommended installation:

```powershell
wsl --install -d Ubuntu
```

Recommended updates:

```bash
sudo apt update
sudo apt upgrade -y
```

Recommended verification:

```powershell
wsl -l -v
```

Expected result:

```text
Ubuntu    Running or Stopped    2
```

Ye confirm karta hai ke Ubuntu successfully WSL 2 par install ho chuka hai.