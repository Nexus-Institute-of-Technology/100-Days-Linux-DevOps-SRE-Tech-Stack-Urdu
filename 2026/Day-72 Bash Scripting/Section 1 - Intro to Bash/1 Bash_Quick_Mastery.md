# MODULE 12 – Systematic Practice Lab: Introduction to Bash Scripting
> **Hands-on Student Lab – Creating, Running, Documenting, and Managing Bash Scripts**

**Suggested Duration:** 60–90 minutes  
**Difficulty Level:** Beginner  
**Environment:** Rocky Linux 9, RHEL 9, Ubuntu, Debian, or another Bash-based Linux system

---

# 🎯 Lab Objectives

By the end of this practice lab, students will be able to:

- Explain the difference between a terminal, console, and shell.
- Identify the current login shell.
- View the shells installed on a Linux system.
- Create a dedicated directory for Bash scripts.
- Create and edit a simple shell script.
- Run a script explicitly with `bash`.
- Add a Bash Shebang line.
- Add execute permission with `chmod`.
- Run a Bash script directly using `./`.
- Add meaningful comments to a Bash script.
- Understand how the `PATH` environment variable works.
- Temporarily and permanently add a scripts directory to `PATH`.
- Troubleshoot common Bash script execution problems.

---

# 📖 Background Notes

## Terminal

A **terminal** is the interface where a user types commands and views output.

Examples include GNOME Terminal, Windows Terminal, PuTTY, MobaXterm, macOS Terminal, and a Xen Orchestra VM console.

The terminal itself does not interpret Linux commands.

## Console

Historically, a **console** was a physical device connected directly to a computer or server. Today, the words **console** and **terminal** are often used interchangeably.

## Shell

A **shell** is a command-line interpreter. It reads commands, interprets them, starts the required programs, and returns output through the terminal.

Common Linux shells include:

```text
sh
bash
dash
zsh
ksh
csh
```

Bash means:

```text
Bourne Again Shell
```

---

# Command Processing Flow

```text
User
  │
  ▼
Terminal
  │
  ▼
Shell
  │
  ▼
Linux Kernel
  │
  ▼
Program Executes
  │
  ▼
Output Returns to Terminal
```

---

# 🔧 Lab Requirements

Students need:

- A Linux user account.
- Terminal or SSH access.
- A text editor such as `vim`, `vi`, or `nano`.
- Permission to create files in their home directory.

Verify the current user:

```bash
whoami
```

Verify the current directory:

```bash
pwd
```

Verify the home directory:

```bash
echo $HOME
```

---

# LAB 1 – Identify the Current Shell

## Step 1: Display the Login Shell

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

## Step 2: Display the Current Shell Process

```bash
ps -p $$ -o pid,ppid,comm,args
```

Here, `$$` is the PID of the current shell.

## Step 3: View Installed Shells

```bash
cat /etc/shells
```

Count valid shell entries:

```bash
grep -vE '^[[:space:]]*(#|$)' /etc/shells | wc -l
```

---

# LAB 2 – Create a Scripts Directory

Return to your home directory:

```bash
cd
```

Create the scripts directory:

```bash
mkdir -p scripts
```

Enter it:

```bash
cd scripts
```

Verify:

```bash
pwd
ls -la
```

---

# LAB 3 – Create the First Shell Script

Create the script:

```bash
vi hello.sh
```

Enter:

```bash
echo "Hello, Friend!"
```

Save and exit.

View it:

```bash
cat hello.sh
```

Run it with Bash:

```bash
bash hello.sh
```

Expected output:

```text
Hello, Friend!
```

Also test:

```bash
sh hello.sh
```

---

# LAB 4 – Convert the File into a Bash Script

Edit the file:

```bash
vi hello.sh
```

Change it to:

```bash
#!/bin/bash

echo "Hello, Friend!"
```

The first line is the **Shebang**. It tells Linux to use `/bin/bash` to interpret the script.

Verify:

```bash
head -n 1 hello.sh
```

Expected output:

```bash
#!/bin/bash
```

---

# LAB 5 – Make the Script Executable

Check permissions:

```bash
ls -l hello.sh
```

Try running it directly:

```bash
./hello.sh
```

You may receive:

```text
Permission denied
```

Add execute permission for the owner:

```bash
chmod u+x hello.sh
```

Verify:

```bash
ls -l hello.sh
```

Run it:

```bash
./hello.sh
```

Expected output:

```text
Hello, Friend!
```

## Permission Reference

| Symbol | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |
| `u` | User or owner |
| `g` | Group |
| `o` | Others |
| `a` | All users |

---

# LAB 6 – Add Comments to the Script

Edit the script:

```bash
vi hello.sh
```

Use:

```bash
#!/bin/bash

# Script Name: hello.sh
# Purpose: Display a greeting and the current date.
# Author: Student Name
# Date: July 14, 2026

# Display a greeting message.
echo "Hello, Friend!"

# Display the current date and time.
date
```

Run:

```bash
./hello.sh
```

Comments begin with `#` and are ignored by Bash, except for the Shebang line.

---

# LAB 7 – Add More System Information

Update `hello.sh`:

```bash
#!/bin/bash

# Display a greeting and basic system information.

echo "Hello, Friend!"
echo "User: $(whoami)"
echo "Hostname: $(hostname)"
echo "Current Directory: $(pwd)"
echo "Date: $(date)"
```

Run:

```bash
./hello.sh
```

The syntax `$(command)` is called **command substitution**.

---

# LAB 8 – Understand Why `hello.sh` Is Not Found

Run:

```bash
hello.sh
```

Expected error:

```text
command not found
```

The script exists, but Bash does not automatically search the current directory.

This works:

```bash
./hello.sh
```

The dot (`.`) means the current directory.

The absolute path also works:

```bash
$HOME/scripts/hello.sh
```

---

# LAB 9 – Examine the `PATH` Variable

Display `PATH`:

```bash
echo "$PATH"
```

Display one directory per line:

```bash
echo "$PATH" | tr ':' '\n'
```

The colon (`:`) separates directories. Bash searches them from left to right.

---

# LAB 10 – Temporarily Add the Scripts Directory to `PATH`

Run:

```bash
export PATH="$PATH:$HOME/scripts"
```

Verify:

```bash
echo "$PATH" | tr ':' '\n'
```

Run the script by name:

```bash
hello.sh
```

Locate it:

```bash
command -v hello.sh
```

Expected pattern:

```text
/home/username/scripts/hello.sh
```

---

# LAB 11 – Test the Script from Another Directory

Move to `/tmp`:

```bash
cd /tmp
```

Run:

```bash
hello.sh
```

It should work because `$HOME/scripts` is in `PATH`.

---

# LAB 12 – Make the `PATH` Change Permanent

Back up `.bashrc`:

```bash
cp "$HOME/.bashrc" "$HOME/.bashrc.backup"
```

Add the scripts directory:

```bash
echo 'export PATH="$PATH:$HOME/scripts"' >> "$HOME/.bashrc"
```

Reload the configuration:

```bash
source "$HOME/.bashrc"
```

Verify:

```bash
command -v hello.sh
hello.sh
```

> Avoid adding the current directory with `export PATH=.:$PATH`; this can create a security risk.

---

# LAB 13 – Create a System Information Script

Create:

```bash
nano "$HOME/scripts/system-info.sh"
```

Add:

```bash
#!/bin/bash

# Display basic system information.

echo "================================="
echo "       SYSTEM INFORMATION"
echo "================================="
echo "User: $(whoami)"
echo "Hostname: $(hostname)"
echo "Kernel: $(uname -r)"
echo "Current Date: $(date)"
echo "Current Directory: $(pwd)"
echo "================================="
```

Make it executable:

```bash
chmod u+x "$HOME/scripts/system-info.sh"
```

Run:

```bash
system-info.sh
```

---

# LAB 14 – Student Challenge: CPU Core Script

Create `cores.sh` with these requirements:

- Add a Bash Shebang.
- Add comments.
- Display the number of CPU cores.
- Make it executable.
- Run it from any directory.

Useful command:

```bash
nproc
```

Expected format:

```text
This system has 4 CPU core(s).
```

Suggested solution:

```bash
#!/bin/bash

# Display the number of available CPU cores.

echo "This system has $(nproc) CPU core(s)."
```

---

# LAB 15 – Student Challenge: Greeting Script

Create `greet.sh` with these requirements:

1. Ask the user for a name.
2. Store the answer in a variable.
3. Greet the user.
4. Display the current date.
5. Include at least three comments.

Suggested starting point:

```bash
#!/bin/bash

# Ask the user for a name.
read -p "Please enter your name: " name

# Display a personalized greeting.
echo "Hello, $name!"

# Display the current date.
echo "Today is $(date)"
```

---

# 🧪 Knowledge Check

1. What is the difference between a terminal and a shell?
2. What is the purpose of `#!/bin/bash`?
3. Why does `hello.sh` fail while `./hello.sh` works?
4. Which command adds execute permission to the owner?
5. Which command displays the current login shell?
6. Which command lists installed valid shells?
7. What does the colon (`:`) represent inside `PATH`?
8. Which file commonly stores permanent Bash configuration for the user?

---

# 🔧 Troubleshooting Guide

## Problem 1 – Permission Denied

```bash
chmod u+x hello.sh
```

## Problem 2 – Command Not Found

```bash
./hello.sh
```

or:

```bash
export PATH="$PATH:$HOME/scripts"
```

## Problem 3 – Bad Interpreter

Check:

```bash
head -n 1 hello.sh
```

Correct:

```bash
#!/bin/bash
```

Check line endings:

```bash
file hello.sh
```

Convert Windows line endings if needed:

```bash
sed -i 's/\r$//' hello.sh
```

## Problem 4 – Script Displays No Output

```bash
cat hello.sh
bash -x hello.sh
```

## Problem 5 – PATH Change Disappears

Add it to `~/.bashrc` and reload:

```bash
source ~/.bashrc
```

## Problem 6 – Works with `bash hello.sh` but Not `./hello.sh`

Check permissions and Shebang:

```bash
ls -l hello.sh
head -n 1 hello.sh
```

---

# ✅ Student Submission Checklist

Students should submit the output of:

```bash
pwd
echo "$SHELL"
cat /etc/shells
ls -l "$HOME/scripts"
cat "$HOME/scripts/hello.sh"
cat "$HOME/scripts/system-info.sh"
command -v hello.sh
hello.sh
system-info.sh
```

---

# 📊 Instructor Assessment Rubric

| Task | Points |
|---|---:|
| Correct scripts directory created | 5 |
| Valid Shebang used | 10 |
| Script contains working commands | 15 |
| Execute permission configured | 10 |
| Script runs with `./` | 10 |
| Comments included | 10 |
| Scripts directory added temporarily to `PATH` | 10 |
| Permanent PATH configuration completed | 10 |
| `cores.sh` challenge completed | 10 |
| Troubleshooting questions answered | 10 |
| **Total** | **100** |

---

# 📌 Quick Revision

| Command | Purpose |
|---|---|
| `echo "$SHELL"` | Display the login shell |
| `cat /etc/shells` | List valid installed shells |
| `mkdir -p ~/scripts` | Create the scripts directory |
| `bash hello.sh` | Run a script explicitly with Bash |
| `#!/bin/bash` | Select Bash as the interpreter |
| `chmod u+x hello.sh` | Add execute permission to the owner |
| `./hello.sh` | Run a script from the current directory |
| `echo "$PATH"` | Display the command search path |
| `export PATH="$PATH:$HOME/scripts"` | Temporarily add the scripts directory |
| `source ~/.bashrc` | Reload Bash configuration |
| `command -v hello.sh` | Show the command Bash will execute 
- Comments improve readability and documentation.
- Permanent Bash settings are commonly stored in `~/.bashrc`.

---

# 💡 Remember

> **Think of Bash scripting as creating reusable instructions for Linux.**
>
> - The **terminal** is where you provide the instructions.
> - The **shell** understands the instructions.
> - The **script** stores the instructions.
> - The **Shebang** selects the correct interpreter.
> - Execute permission allows Linux to run the script.
> - `PATH` tells Bash where scripts and commands are located.

```text
Write Script
     │
     ▼
Add Shebang
     │
     ▼
Add Execute Permission
     │
     ▼
Run and Test
     │
     ▼
Add to PATH
     │
     ▼
Use as a Normal Command
```|
| `bash -x hello.sh` | Run a script in debugging mode |

---

# 📖 Key Takeaways

- A terminal is the interface where commands are entered.
- A shell interprets and executes commands.
- Bash is the most commonly used Linux shell.
- A Bash script is a text file containing commands.
- The Shebang selects the script interpreter.
- Execute permission is required to run a script directly.
- `./` explicitly references the current directory.
- `PATH` contains directories where Bash searches for commands.
- Adding `$HOME/scripts` to `PATH` allows scripts to run like normal commands.