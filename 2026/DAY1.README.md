# NIT Academy Mein Khush Amdeed - Din 1 or 2

## Fehrist

| Task | Unwan | Khulasi |
|------|------|------|
| 1 | [Apne Virtual Machine Tak Rasai Hasil Karein](#task-1---apne-virtual-machine-tak-rasai-hasi) | Unix/Linux ka taaruf |
| 2 | [Linux Shell Kya Hai?](#task-2---linux-shell-kya-hai) | Virtual machine mein login karein aur Linux command line (CLI) tak rasai hasil karein |
| 3 | [GitHub Account Register Karein](#task-3---github-account-register-karein) | Apna GitHub account banayein |
| 4 | [LinkedIn Register Karein Ya Update Karein](#task-4---linkedin-register-karein-ya-update-karein) | LinkedIn profile banayein ya behtar banayein |
| 5 | [Git Bash Install Karein](#task-5---git-bash-install) | Windows par Git Bash install karein |
| 6 | [Visual Studio Code](#task-6---visual-studio-code) | Visual Studio Code install karein |
| 7 | [Discord](#task-7---discord) | Discord install karein aur shamil hoen |

---

# Hum Ab Apna Linux Safar shuru kar rahe hain
Isay sajagti se lein aur tamam assignments waqt par mukammal karein.

# Task 1 - Apne Virtual Machine Tak Rasai Hasil Karein?
1. Xen-Orchestra Tak Rasai
Neeche diye gaye link par click karein taake login page tak rasai hasil karein\
[Xen-Orchestra](https://labs.nit.academy)

<img src=".././.github/assets/XO-LOGIN.jpg" width="500">\
Apne credentials andarkarein:
```Text
Username:
Password:
```
2. Apne Virtual Machine (VM) Console Par Navigate Karein
Is virtual machine par click karein jismein "sabz" dot ho:\
<img src=".././.github/assets/XO-VM.jpg" width="700">

Apne Virtual Machine ke resource ka jaizza lein:\
<img src=".././.github/assets/XO-VM-CONSOLE.jpg" width="700">

"Console" tab par click karein:\
<img src=".././.github/assets/XO-LINUX-VM.jpg" width="700">
```Text
Marhala 1 - "Siyah Box" par click karein
Marhala 2 - "Enter Key" dabayein
```
<img src=".././.github/assets/XO-VM-LOGIN.jpg" width="700">

```bash
   unknowna1alsdjfals5 Login: root
   Password: abc
   Last login: Sat Apr 18 02:51:09 on tty1
```
3. Aiyai is IPv6 overflow ke masle ko theek karte hain.
Jab hum networking sikhenge to hum samjhayenge:
- IP4 vs IP6
- Static IP ki ahmiyat
- Networking maslon ka azala

## Aiyai hum is maslay ko theek karain. In commands ko aap apni virtual machine mai type karain:

```bash
sysctl -w net.ipv6.conf.all.disable_ipv6=1
```
```bash
sysctl -w net.ipv6.conf.default.disable_ipv6=1
```
### Task is now complete!
```
### Task Ab Mukammal ho gaya!

# Task 2 - Linux Shell Kya Hai?

1. **Shell (Interface)**  
   Shell ko operating system ki "bahiri tah" ki tarah samjhein (jaisay kisi mitti ke bahiri khol ki tarah). Yeh wo mahol hai jahan aap apne hidayat type karte hain.

   **Analogy:**  
   Shell aap aur computer ke darmiyan chat window ki tarah hai. Aap kuch type karte hain, aur yeh computer ke jawaab ka intezar karta hai.

2. **Interpreter (Translator)**
   Interpreter wo program hai jo shell ke andar chal raha hai. Computer English nahi bolti; wo binary (1s aur 0s) bolti hai. Interpreter wo alfaiz jo aap type karte hain aur unhen us zaban mein tarjuma karta hai jo computer ka "dimagh" (kernel) samjhta hai.

   **Analogy:**  
   Interpreter do logon ke darmiyan baitha hua tarjuman hai jo alag alag zuban bolte hain.

3. **Command Line (Action)**
   Command line bas wo khaas line hai jise aap kaam mukammal karne ke liye type karte hain. Iska aam taur par sadah "Verb + Object" structur hota hai.

   Misal:
   ```bash
   <root@vm1>#whoami
   ```
   ## "Pehle Teerh" Commands

   | Command | Yeh Kya Karta Hai | Is Ki Ahmiyat |
   |----------|---------------|----------------|
   | `whoami` | Aap ka username dikhata hai. | Tasdiq karta hai ke system se kon baat kar raha hai. |
   | `pwd` | **Print** Working Directory. | Batata hai ke aap folder system mein kahan hain. |
   | `ls` | Files **List** karta hai. | Dikhata hai ke aap ke maujuda folder mein kya hai. |
   | `date` | Maujuda waqt/tareekh dikhata hai. | Sadah buhot hai ke **Interpreter** kaam kar raha hai. |
   | `clear` | screen saaf karta hai. | "Command line inequality" kam karne ke liye zaroori. |
   | `ip a` | Network interface aur IP address dikhata hai. | Network par apne machine ki pehchan karne mein madad karta hai. |
   | `hostnamectl` | System ka hostname aur operating system ki tafseelat dikhata hai. | Machine aur system ki information ki pehchan mein madad karta hai. |
   | `hostnamectl set-hostname <your-hostname>` | System ka hostname tabdeel karta hai. | servers ko naam dene aur network ya lab mein systems ko munqasam karne ke liye mofeed hai. |
   | `uptime` | Dikhata hai ke system kitne waqt se chal raha hai. | System ki istqamat aur bojha ki monitoring mein madad karta hai. |
   | `reboot` | Operating system ko dobara shuru karta hai. | Tabdiliyan lagoo karne ya bahali ke liye aam idari command hai. |
   | `passwd` | User ka password tabdeel karta hai. | Account ki hifazat aur user ke idari ke liye zaroori hai. |
   | `/bin/bash` | Naya Bash shell launch karta hai (aam taur par non-login shell). | Shell mein dakhil hone ya fault ke azala ke liye mofeed hai. |
   | `exit` | Maujuda shell session band karta hai. | Mehfooz taur se log out hota hai ya terminal session se nikalta hai. |
     
# Task 3 - GitHub Account Register Karein
### GIT ka matlab hai "Global Information Tracker"
Is link par click karein https://github.com/
## GitHub Account Setup Ke Marhale

| Marhala | Amal | Tafseelat |
|------|---------|-------------|
| 1 | GitHub website kholein | https://github.com par jayein |
| 2 | Sign up par click karein | Home page par **Sign up** button dabayein |
| 3 | malumat andarkarein | Apna email, username, aur password shamil karein |
| 4 | Email ki tasdiq karein | Apna email kholein aur tasdiq link par click karein |
| 5 | Profile mukammal karein | Zaroorat ho to profile ki tafseelat shamil karein |
| 6 | Login karein | Apnay naye GitHub account mein sign in karein |
| 7 | Repository banayein | **+** par click karein → **New repository** |
| 8 | Repository ka naam shamil karein | Project/repository ka naam andarkarein |
| 9 | README shamil karein | **Add README file** check karein |
| 10 | Repository banayein | **Create repository** par click karein |

# Task 4 - LinkedIn Register Karein Ya Update Karein
Is link par click karein https://www.linkedin.com/\
LinkedIn par apna account banayein

# Task 5 - Git Bash
Is link par click karein https://git-scm.com/install/windows
## Git Bash Setup Ke Marhale

| Marhala | Amal | Tafseelat |
|------|---------|-------------|
| 1 | Git Bash download karein | https://git-scm.com/download/win par jayein |
| 2 | Installer chalaein | Download kiya hua `.exe` file kholein |
| 3 | Installation shuru karein | Setup mein **Next** par click karein |
| 4 | PATH option ka intikhab karein | Intikhab karein: **Git from command line and also from 3rd-party software** |
| 5 | HTTPS backend | Intikhab karein: **Use the OpenSSL library** |
| 6 | Line ending option | Intikhab karein: **Checkout Windows-style, commit Unix-style line endings** |
| 7 | Git install karein | **Install** par click karein |
| 8 | Setup mukammal karein | **Finish** par click karein |
| 9 | Git Bash kholein | Start menu se Git Bash ko launch karein |
| 10 | Installation ki tasdiq karein | `git --version` chalaein |
| 11 | Username configure karein | `git config --global user.name "Your Name"` chalaein |
| 12 | Email configure karein | `git config --global user.email "you@example.com"` chalaein |
| 13 | Configuration ki tasdiq karein | `git config --list` chalaein |
| 14 | SSH key generate karein | `ssh-keygen -t ed25519 -C "you@example.com"` chalaein |
| 15 | SSH ko GitHub mein shamil karein | Public key ko GitHub SSH settings mein copy karein |
| 16 | Connection test karein | `ssh -T git@github.com` chalaein |

# Task 6 - Visual Studio Code
Is link par click karein https://code.visualstudio.com/download

# Task 7 - Discord
!! Intihai Ahmiyat !!

Jaisay pehle bataya gaya, NIT ke tamam logon ke liye Discord server taiyaar hai! Is link ke zariye shamil hoen: https://discord.gg/UeVHUzz25A

Yeh bhi note karein ke hum regular taur se WhatsApp ke saath saath communication ke ek aur tariqe ke taur par Discord ka bhi istimal karenge.

# Hatmi Khulasi
Aaj aap ne sikhaya:

- Virtual machine tak rasai hasil karna
- Linux shell ki buniyadi baatein
- Bunyadi Linux commands
- GitHub accounts banana
- Git Bash install karna
- VS Code install karna
- Discord setup karna

NIT Academy mein apne Linux safar mein khush amdeed!