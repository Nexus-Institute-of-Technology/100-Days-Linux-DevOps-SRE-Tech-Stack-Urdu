# Bash Practice Lab: Print the Number of CPU Cores

## Lab Overview

In this lab, you will create a simple Bash script that displays the number of CPU processing units available on a Linux system.

You will use the `nproc` command inside a Bash script named `cores.sh`.

---

## Learning Objectives

By the end of this lab, students will be able to:

- Create a Bash script.
- Add a shebang line.
- Use the `nproc` command.
- Store command output in a variable.
- Make a script executable.
- Run and verify a Bash script.

---

## Estimated Time

**10–15 minutes**

---

## Prerequisites

Students should have:

- Access to a Linux terminal.
- Basic knowledge of Linux commands.
- Permission to create files in their home directory.
- A text editor such as `vi`, `vim`, or `nano`.

---

## Scenario

A system administrator needs to quickly determine how many CPU processing units are available on a Linux server.

Instead of running the command manually every time, you will create a reusable Bash script named:

```text
cores.sh
```

The script must print the number of CPU processing units available on the system.

---

## Task 1: Open the Terminal

Log in to your Linux system and confirm your current user:

```bash
whoami
```

Check your current directory:

```bash
pwd
```

Move to your home directory:

```bash
cd ~
```

---

## Task 2: Test the `nproc` Command

Before writing the script, run the following command:

```bash
nproc
```

Example output:

```text
4
```

The number may be different on your system.

> **Note:** `nproc` reports the number of processing units available to the current system or process. On many systems, this corresponds to the number of logical CPU cores.

---

## Task 3: Create the Bash Script

Create a file named `cores.sh`:

```bash
vi cores.sh
```

Press `i` to enter insert mode.

Add the following script:

```bash
#!/bin/bash

echo "Number of CPU cores:"
nproc
```

Save and exit from `vi`:

1. Press `Esc`.
2. Type `:wq`.
3. Press `Enter`.

---

## Task 4: View the Script

Use `cat` to confirm that the script was saved correctly:

```bash
cat cores.sh
```

Expected content:

```bash
#!/bin/bash

echo "Number of CPU cores:"
nproc
```

---

## Task 5: Check the File Permissions

Run:

```bash
ls -l cores.sh
```

Example output:

```text
-rw-r--r--. 1 student student 58 Jul 17 10:00 cores.sh
```

The script does not yet have execute permission.

---

## Task 6: Make the Script Executable

Add execute permission:

```bash
chmod +x cores.sh
```

Check the permissions again:

```bash
ls -l cores.sh
```

Example output:

```text
-rwxr-xr-x. 1 student student 58 Jul 17 10:00 cores.sh
```

The `x` permissions show that the file is executable.

---

## Task 7: Run the Script

Execute the script:

```bash
./cores.sh
```

Example output:

```text
Number of CPU cores:
4
```

Your number may be different depending on the system.

---

# Improved Solution Using a Variable

The output of a command can be stored in a variable using command substitution.

Edit the script:

```bash
vi cores.sh
```

Replace its contents with:

```bash
#!/bin/bash

cpu_cores=$(nproc)

echo "This system has $cpu_cores CPU core(s)."
```

Save the file and run it again:

```bash
./cores.sh
```

Example output:

```text
This system has 4 CPU core(s).
```

---

## Explanation of the Script

### Shebang

```bash
#!/bin/bash
```

This tells Linux to use the Bash shell to execute the script.

### Variable Assignment

```bash
cpu_cores=$(nproc)
```

This runs the `nproc` command and stores its output in the variable named `cpu_cores`.

### Displaying the Result

```bash
echo "This system has $cpu_cores CPU core(s)."
```

The `$cpu_cores` expression displays the value stored in the variable.

---

# Verification

## Method 1: Use `nproc`

```bash
nproc
```

The result should match the number printed by the script.

---

## Method 2: Use `/proc/cpuinfo`

```bash
grep -c "^processor" /proc/cpuinfo
```

Example output:

```text
4
```

This counts the processor entries shown by the Linux kernel.

---

## Method 3: Use `lscpu`

```bash
lscpu
```

To display only the total number of logical CPUs:

```bash
lscpu | grep "^CPU(s):"
```

Example output:

```text
CPU(s):                                  4
```

---

# Student Practice Tasks

Complete the following tasks without copying the final solution.

## Practice Task 1

Create a script named `cores.sh` that prints only the number returned by `nproc`.

Expected style of output:

```text
4
```

---

## Practice Task 2

Modify the script so that it prints a complete sentence.

Expected style of output:

```text
This system has 4 CPU core(s).
```

---

## Practice Task 3

Add the hostname to the script.

Useful command:

```bash
hostname
```

Expected style of output:

```text
Hostname: student-server
CPU cores: 4
```

---

## Practice Task 4

Add the current username to the script.

Useful command:

```bash
whoami
```

Expected style of output:

```text
User: student
Hostname: student-server
CPU cores: 4
```

---

# Final Challenge

Create a Bash script named `system_cpu_report.sh` that displays:

- Current username
- System hostname
- Operating system name
- Number of available CPU processing units

Useful commands:

```bash
whoami
hostname
uname -o
nproc
```

Expected style of output:

```text
System CPU Report
-----------------
User: student
Hostname: student-server
Operating System: GNU/Linux
CPU cores: 4
```

---

# Final Challenge Solution

```bash
#!/bin/bash

current_user=$(whoami)
system_hostname=$(hostname)
operating_system=$(uname -o)
cpu_cores=$(nproc)

echo "System CPU Report"
echo "-----------------"
echo "User: $current_user"
echo "Hostname: $system_hostname"
echo "Operating System: $operating_system"
echo "CPU cores: $cpu_cores"
```

Make the script executable:

```bash
chmod +x system_cpu_report.sh
```

Run it:

```bash
./system_cpu_report.sh
```

---

# Troubleshooting

## Problem: Permission Denied

Example error:

```text
bash: ./cores.sh: Permission denied
```

Solution:

```bash
chmod +x cores.sh
./cores.sh
```

---

## Problem: Command Not Found

Example error:

```text
cores.sh: command not found
```

Run the script with `./`:

```bash
./cores.sh
```

The `./` tells Linux to run the script from the current directory.

---

## Problem: Bad Interpreter

Example error:

```text
bad interpreter: No such file or directory
```

Confirm that the first line is exactly:

```bash
#!/bin/bash
```

Check that Bash exists:

```bash
ls -l /bin/bash
```

---

## Problem: Script Shows an Unexpected Number

The system may use:

- Multiple physical CPUs.
- Multiple cores per CPU.
- Hyper-threading.
- Virtual CPUs assigned to a virtual machine.
- Container or process CPU restrictions.

The `nproc` command shows the number of processing units available to the current environment.

---

# Knowledge Check

1. What is the purpose of the shebang line?
2. What command displays the number of available CPU processing units?
3. What does `chmod +x cores.sh` do?
4. Why is `./` used before the script name?
5. What does `$(nproc)` mean?
6. How do you display the value of the variable `cpu_cores`?
7. What command can be used to display detailed CPU information?

---

# Knowledge Check Answers

1. It tells Linux which interpreter should execute the script.
2. `nproc`
3. It adds execute permission to the script.
4. It tells Linux to execute the script from the current directory.
5. It runs `nproc` and returns its output.
6. Use `$cpu_cores`.
7. `lscpu`

---

# Lab Completion Checklist

- [ ] I created `cores.sh`.
- [ ] I added the Bash shebang.
- [ ] I used the `nproc` command.
- [ ] I made the script executable.
- [ ] I ran the script successfully.
- [ ] I stored command output in a variable.
- [ ] I verified the CPU count using another command.
- [ ] I completed the final challenge.

---

# Instructor Validation Commands

The instructor can check the student's script with:

```bash
ls -l cores.sh
cat cores.sh
./cores.sh
```

A valid script should:

1. Be named `cores.sh`.
2. Be executable.
3. Contain a valid Bash shebang.
4. Use the `nproc` command.
5. Display the CPU processing-unit count without errors.

---

# Cleanup

Remove the practice scripts when they are no longer needed:

```bash
rm -i cores.sh system_cpu_report.sh
```

Confirm that they were removed:

```bash
ls -l cores.sh system_cpu_report.sh
```

Expected result:

```text
ls: cannot access 'cores.sh': No such file or directory
ls: cannot access 'system_cpu_report.sh': No such file or directory
```