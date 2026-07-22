# NEXUS VENTURES: Linux Process Management Project — Root Account Edition

## Project Title

**Linux Process Operations Center: Monitoring, Controlling, and Troubleshooting Processes**

---

## 1. Project Overview

In this project, you will build and troubleshoot a small Linux process-management environment on a Rocky Linux system.

You will create several scripts and programs that simulate:

- A long-running application
- Foreground and background jobs
- A CPU-intensive process
- A process that continues after logout
- Parent and child processes
- A zombie process
- A systemd-managed service
- A production-style process incident

By completing this project, you will learn how Linux administrators monitor, control, prioritize, stop, restart, and troubleshoot processes.

> **Lab account:** Every command assumes that the student is logged in directly as `root`. Therefore, no command uses elevated-command prefixes.

---

## 2. Learning Objectives

At the end of this project, you should be able to:

1. Explain the meaning of PID and PPID.
2. Identify process owners and process states.
3. View processes with `ps`, `top`, `pgrep`, and `pstree`.
4. Run commands in the foreground and background.
5. Manage shell jobs with `jobs`, `bg`, and `fg`.
6. Send signals with `kill` and `pkill`.
7. Explain the difference between `SIGTERM` and `SIGKILL`.
8. Pause and resume a process.
9. Change process priority with `nice` and `renice`.
10. Keep a process running after logout with `nohup`.
11. Inspect process information through `/proc`.
12. Identify a zombie process.
13. Create and manage a systemd service.
14. Troubleshoot a high-CPU process.
15. Validate that a failed service has been restored.

---

## 3. Lab Requirements

### Recommended virtual machine

- Rocky Linux 9
- 1 or 2 vCPUs
- 1 GB RAM or more
- 10 GB disk space or more
- A root account
- Students logged in directly as `root`
- SSH access or console access
- Internet access for installing packages

### Required packages

Install the tools used in this project:
---
These three Linux packages serve distinct roles across **system administration**, **process management**, and **software development**.

---

## 1. `procps-ng` (Process Management Utilities - Next Generation)

`procps-ng` provides command-line utilities that read from the pseudo-filesystem (`/proc`) to give you real-time information about system resources, memory usage, and running processes.

### Core Tools Included
* **`ps`**: Reports a snapshot of all currently running processes.
* **`top`**: Displays an interactive, real-time list of active system processes and CPU/RAM usage.
* **`free`**: Shows used, free, and cached system RAM and swap memory.
* **`uptime`**: Shows how long the machine has been running along with load averages.
* **`pgrep` / `pkill`**: Search for or send signals to processes based on names or user attributes.
* **`sysctl`**: Configures Linux kernel parameters dynamically at runtime.
* **`vmstat`**: Reports system memory, swap, I/O, and CPU statistics.
* **`watch`**: Runs a specified command repeatedly at regular intervals to monitor its output in full screen.

---

## 2. `psmisc` (Miscellaneous Process Utilities)

`psmisc` contains lightweight, specialized tools for inspecting and controlling processes based on filenames, process trees, or executable names.

### Core Tools Included
* **`pstree`**: Displays all active processes formatted as a visual hierarchical tree (showing parent and child relationships).
* **`killall`**: Terminates all processes matching a given command name (e.g., `killall firefox`).
* **`fuser`**: Displays the Process IDs (PIDs) using specified files, directories, or network sockets.
* **`peekfd`**: Inspects data transfer taking place on open file descriptors of a targeted process.

---

## 3. `gcc` (GNU Compiler Collection)

`gcc` is the standard open-source compiler toolchain on Linux systems. It translates raw source code into executable binary programs or system libraries.

### Core Capabilities Included
* **C and C++ Compilers**: Includes `gcc` (C compiler driver) and `g++` (C++ compiler driver).
* **Build Architecture Support**: Handles preprocessing (`cpp`), optimization, machine code generation, and linking against runtime C/C++ libraries (such as `glibc`).
* **System Capabilities**: Essential for compiling software from source code (`.c` / `.cpp` files), building Linux kernel modules, and installing C-extension dependencies for languages like Python or Node.js.

---

### Quick Comparison Summary

| Package | Primary Purpose | Example Use Case |
| :--- | :--- | :--- |
| **`procps-ng`** | General system & process monitoring | Checking free memory (`free -m`) or CPU utilization (`top`). |
| **`psmisc`** | Targeted process manipulation | Finding what process is locking a directory (`fuser /mnt`) or killing an app by name (`killall app`). |
| **`gcc`** | Code compilation toolchain | Compiling a C program into a binary executable (`gcc main.c -o app`). |

---

```bash
dnf install -y procps-ng psmisc gcc tree
```

Verify the packages:

```bash
rpm -q procps-ng psmisc gcc tree
```

You should see the installed package versions.

---

## 4. Root Account Requirement

All commands in this project are written for students who are logged in directly as `root`.

Verify the current account before beginning:

```bash
whoami
```

Expected result:

```text
root
```

You can also verify the effective user ID:

```bash
id
```

The output should include:

```text
uid=0(root)
```
### Change the name of your VM:
```bash
hostnamectl set-hostname Project-01-Process
exec bash
```

Do not continue until you have confirmed that you are working inside your assigned lab VM and are logged in as `root`.


---

## 5. Important Safety Rules

This project intentionally creates CPU-intensive and long-running processes.

Follow these rules:

1. Perform the project only on a lab virtual machine.
2. Do not run the CPU worker on a production server.
3. Run only one CPU worker unless instructed otherwise.
4. Record the PID before terminating a process.
5. Try `SIGTERM` before using `SIGKILL`.
6. Complete the cleanup section after finishing the lab.

---
## BEFORE YOU START
You are required for this project to record all the linux activity you will be doing by using a Linux utility called **`script`**! 

It's built into virtually every Linux distribution (part of the `util-linux` package), and its exact job is to record everything that happens in your terminal session—both what you type and the system's output—into a log file.

Here is the exact step-by-step workflow to do this cleanly during your patching window.

---

## Step-by-Step Patching Guide Using `script`

### 1. Start the Recording Session
1. Before running any patch or update commands, launch `script` with a clear filename.
2. By default, the .log file is saved in your current working directory—which is whichever folder you were sitting in when you ran the script command.
3. You can pass an absolute path to script so you know exactly where it goes, regardless of your current directory:
For this project do the following:

```bash
script process_management_linux_project_$(date +%Y%m%d).log
---
## 6. Project Directory Setup

Create the project directory:

```bash
mkdir -p /opt/process-lab
```

Set root as the initial owner:

```bash
chown root:root /opt/process-lab
```

Enter the directory:

```bash
cd /opt/process-lab
```

Verify:

```bash
pwd
ls -ld /opt/process-lab
```

Expected directory:

```text
/opt/process-lab
```

---

# Phase 1: Create the Report Generator Application

## 6. Create the Application Script
This script is a simple, infinite background logging process designed to simulate an ongoing system service (like a report generator).
- Key Characteristics & Behavior
- Resource Impact: Extremely lightweight. It spends 99.9% of its time sleeping and negligible CPU/memory to append text.
- Log Growth: The log file at /tmp/report-generator.log will grow indefinitely by one line every 10 seconds until the process is killed.
- Termination: Because there is no exit condition inside the while loop, you must stop it externally using Ctrl + C (if running in the foreground) or via kill <PID>.

Create the script:

```bash
vi /opt/process-lab/report-generator.sh
```

Add the following content:

```bash
#!/bin/bash
# Variable Assignment: Defines where the script will record its activity—in a file named report-generator.log located inside the temporary directory (/tmp).
LOG_FILE="/tmp/report-generator.log"

* **`$(date)`**: Inserts the current date and time.
* **`$$`**: Built-in Bash variable for the current Process ID (PID).
* **`$PPID`**: Parent Process ID (the shell/session that started it).
* **`>>`**: Redirects and appends output to the end of the file.

echo "$(date): Report Generator started with PID $$ and PPID $PPID" >> "$LOG_FILE"

# Infinite Loop:
# while true runs indefinitely until the process is manually terminated or killed.
# Every iteration logs a timestamp and message to /tmp/report-generator.log.
# sleep 10 pauses execution for 10 seconds before repeating the cycle.

while true
do
    echo "$(date): PID $$ is generating a report" >> "$LOG_FILE"
    sleep 10
done
```

Save and exit.

Make the script executable:

```bash
chmod +x /opt/process-lab/report-generator.sh
```

Verify its permissions:

```bash
ls -l /opt/process-lab/report-generator.sh
```

Expected result:

```text
-rwxr-xr-x
```

The exact owner, group, date, and file size may be different.

---

## 7. Run the Application in the Foreground (NOT in the background!)

Run:

```bash
cd /opt/process-lab
./report-generator.sh
```

The terminal appears to remain busy because the application is running in the foreground.

Open another SSH session or terminal and run:

```bash
pgrep -af report-generator
```

You may see output similar to:

```text
2456 /bin/bash ./report-generator.sh
```

Check the process with `ps`:

```bash
ps -ef | grep '[r]eport-generator'
```

Display selected process information:

```bash
PID=$(pgrep -f '/opt/process-lab/report-generator.sh|./report-generator.sh' | head -1)
ps -o user,pid,ppid,stat,etime,cmd -p "$PID"
```

### Validation Checkpoint 1

Confirm that:

- The script is running.
- It has a PID.
- It has a PPID.
- The process owner is your user.
- Its elapsed time increases.

Check the log:

```bash
tail -n 5 /tmp/report-generator.log
```

You should see timestamped entries being added approximately every 10 seconds.

Stop the foreground process by returning to its terminal and pressing:

```text
Ctrl+C
```

Validate that it stopped:

```bash
pgrep -af report-generator
```

No report-generator process should be returned.

---

# Phase 2: Foreground and Background Job Management

## 8. Start the Application in the Background

Run:

```bash
cd /opt/process-lab
./report-generator.sh &
```

The shell should display a job number and PID similar to:

```text
[1] 2601
```

In this example:

- `[1]` is the shell job number.
- `2601` is the process ID.

Display background jobs:

```bash
jobs
```

Display job numbers and PIDs:

```bash
jobs -l
```

Expected output resembles:

```text
[1]+ 2601 Running ./report-generator.sh &
```

---

## 9. Move the Job to the Foreground

Run:

```bash
fg %1
```

The job moves into the foreground.

Suspend it by pressing:

```text
Ctrl+Z
```

Expected message:

```text
[1]+ Stopped ./report-generator.sh
```

Check the job:

```bash
jobs -l
```

---

## 10. Resume the Job in the Background

Run:

```bash
bg %1
```

Check:

```bash
jobs -l
```

The job should show `Running`.

### Validation Checkpoint 2
> This two-line snippet is used to inspect a background job running in your current terminal session by looking up its Process ID (PID) and checking its detailed status with ps
---
**Line-by-Line Breakdown**
- Line 1: JOB_PID=$(jobs -p %1)
- **jobs -p %1:** Looks at the shell's active job table and extracts only the Process ID (PID) of Job #1 (the first background job running in that terminal session).

- **JOB_PID=$(...):** Stores that numerical PID into a shell variable named JOB_PID.

- **Line 2:** ps -o pid,ppid,stat,cmd -p "$JOB_PID"
- **-p** "$JOB_PID": Tells ps to query only the specific process stored in your variable.

- -o pid,ppid,stat,cmd: Customizes the output columns to display only what matters:

- **PID:** Process ID of the job.

- **PPID:** Parent Process ID (which will match your terminal shell's PID).

- **STAT:** Current process state (e.g., S for sleeping, R for running, T for stopped, + for foreground).

- **CMD:** The actual command/script name running.
---

Run:

```bash
JOB_PID=$(jobs -p %1)
ps -o pid,ppid,stat,cmd -p "$JOB_PID"
```

The process state may contain:

```text
S
```

This normally means the process is sleeping while waiting for an event or timer.

---

## 11. Stop the Background Job

Terminate it by **job number**:

```bash
kill %1
```

Check:

```bash
jobs -l
```

If the completed job still appears, press Enter or run `jobs` again.

Validate:

```bash
pgrep -af report-generator
```

---

# Phase 3: Linux Process Signals

## 12. Start the Application Again

Run:

```bash
cd /opt/process-lab
./report-generator.sh &
```

Save the PID:

```bash
REPORT_PID=$!
echo "$REPORT_PID"
```

Validate:

```bash
ps -o pid,ppid,stat,cmd -p "$REPORT_PID"
```

---

## 13. Pause the Process with SIGSTOP

Send signal 19, `SIGSTOP`:

```bash
kill -STOP "$REPORT_PID"
```

Check its state:

```bash
ps -o pid,ppid,stat,cmd -p "$REPORT_PID"
```

The process state should contain:

```text
T
```

`T` normally means stopped or suspended.

Check the last log entry:

```bash
tail -n 3 /tmp/report-generator.log
```

Wait at least 15 seconds, then check again:

```bash
sleep 15
tail -n 3 /tmp/report-generator.log
```

No new report entry should have been written while the process was stopped.

---

## 14. Resume the Process with SIGCONT

Run:

```bash
kill -CONT "$REPORT_PID"
```

Check:

```bash
ps -o pid,ppid,stat,cmd -p "$REPORT_PID"
```

Wait and check the log:

```bash
sleep 12
tail -n 5 /tmp/report-generator.log
```

New entries should appear.

---

## 15. Terminate the Process Gracefully with SIGTERM

Run:

```bash
kill -TERM "$REPORT_PID"
```

This is equivalent to:

```bash
kill -15 "$REPORT_PID"
```

Validate:

```bash
ps -p "$REPORT_PID"
```

The process should no longer appear.

### Why SIGTERM is preferred

`SIGTERM` asks the process to terminate. Applications can handle this signal and perform cleanup before exiting.

Examples of cleanup include:

- Closing files
- Writing final log entries
- Completing transactions
- Releasing locks
- Removing temporary files

---

## 16. Forcefully Terminate a Process with SIGKILL

Start the application again:

```bash
./report-generator.sh &
REPORT_PID=$!
```

Forcefully stop it:

```bash
kill -KILL "$REPORT_PID"
```

This is equivalent to:

```bash
kill -9 "$REPORT_PID"
```

Validate:

```bash
ps -p "$REPORT_PID"
```

The process should be gone.

### Important difference

`SIGKILL` cannot be caught, ignored, or handled by the target process. The Linux kernel immediately terminates it.

Use `SIGKILL` only when:

- `SIGTERM` does not work.
- The process is unresponsive.
- Immediate termination is required.

### Validation Checkpoint 3

Record the purpose of these signals:

| Signal | Number | Purpose |
|---|---:|---|
| `SIGTERM` | 15 | Requests graceful termination |
| `SIGKILL` | 9 | Forces immediate termination |
| `SIGSTOP` | 19 | Pauses a process |
| `SIGCONT` | 18 | Resumes a paused process |

---

# Phase 4: Simulate and Troubleshoot High CPU Usage

## 17. Create the CPU Worker
This snippet starts an infinite loop that consumes 100% of a single CPU core without performing any useful work. It is commonly used as a lightweight "stress test" or CPU worker simulation in Linux labs.

Create:

```bash
vi /opt/process-lab/cpu-worker.sh
```

Add:

```bash
#!/bin/bash

echo "CPU worker started with PID $$"

while true
do
    :
done
```

Make it executable:

```bash
chmod +x /opt/process-lab/cpu-worker.sh
```

Validate:

```bash
bash -n /opt/process-lab/cpu-worker.sh
```

No output normally means that Bash did not detect a syntax error.

---

## 18. Start the CPU Worker

Run only one worker:

```bash
cd /opt/process-lab
./cpu-worker.sh &
```

Save its PID:

```bash
CPU_PID=$!
echo "$CPU_PID"
```

Confirm it is running:

```bash
ps -o pid,ppid,stat,%cpu,%mem,ni,cmd -p "$CPU_PID"
```

---

## 19. Identify the High-CPU Process

Run:

```bash
top
```

Inside `top`:

- Press `P` to sort by CPU usage.
- Locate `cpu-worker.sh` or the Bash process running it.
- Press `q` to exit.

You can also use:

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,ni,cmd --sort=-%cpu | head -10
```

Or:

```bash
top -b -n 1 | head -20
```

### Validation Checkpoint 4

Confirm that:

- The CPU worker is near the top of the process list.
- It has a PID.
- It is consuming significant CPU.
- Its default nice value is normally `0`.

---

# Phase 5: Process Priority with nice and renice

## 20. Change the Running Process Priority

View its current priority:

```bash
ps -o pid,ni,pri,%cpu,cmd -p "$CPU_PID"
```

Lower its scheduling priority by increasing its nice value:

```bash
renice 10 -p "$CPU_PID"
```

Verify:

```bash
ps -o pid,ni,pri,%cpu,cmd -p "$CPU_PID"
```

The `NI` value should now be `10`.

### Understanding nice values

- Nice values normally range from `-20` to `19`.
- A lower number gives a process a higher scheduling priority.
- A higher number gives a process a lower scheduling priority.
- Regular users can usually increase the nice value.
- Because you are logged in as `root`, you can increase or decrease a process's nice value.
- In production, changing a process to a higher scheduling priority should be done carefully.

---

## 21. Stop the CPU Worker

Try graceful termination:

```bash
kill -15 "$CPU_PID"
```

Validate:

```bash
ps -p "$CPU_PID"
```

If it remains running, forcefully terminate it:

```bash
kill -9 "$CPU_PID"
```

---

## 22. Start a Process with a Nice Value

Start the worker with nice value `15`:

```bash
nice -n 15 /opt/process-lab/cpu-worker.sh &
```

Save its PID:

```bash
NICE_PID=$!
```

Verify:

```bash
ps -o pid,ni,pri,%cpu,cmd -p "$NICE_PID"
```

Expected `NI` value:

```text
15
```

Stop it:

```bash
kill -15 "$NICE_PID"
```

If necessary:

```bash
kill -9 "$NICE_PID"
```

### Validation Checkpoint 5

Complete this table in your notes:

| Test | Nice value | Observation |
|---|---:|---|
| Default process | 0 | |
| Process changed with `renice` | 10 | |
| Process started with `nice` | 15 | |

---

# Phase 6: Keep a Process Running After Logout

## 23. Run the Application with nohup

Run:

```bash
cd /opt/process-lab
nohup ./report-generator.sh > /tmp/report-output.log 2>&1 &
```

Save the PID:

```bash
NOHUP_PID=$!
echo "$NOHUP_PID"
```

Check:

```bash
ps -o pid,ppid,stat,cmd -p "$NOHUP_PID"
```

Display the output file:

```bash
cat /tmp/report-output.log
```

The script itself writes its operational messages to:

```text
/tmp/report-generator.log
```

---

## 24. Test Logout Survival

Record the PID:

```bash
echo "$NOHUP_PID" | tee /tmp/nohup-report.pid
```

Log out of the SSH session:

```bash
exit
```

Reconnect to the VM.

Read the saved PID:

```bash
NOHUP_PID=$(cat /tmp/nohup-report.pid)
```

Check whether the process survived:

```bash
ps -o pid,ppid,stat,etime,cmd -p "$NOHUP_PID"
```

Also check:

```bash
pgrep -af report-generator
```

### Validation Checkpoint 6

The process should still be running after logout.

Stop it:

```bash
kill -15 "$NOHUP_PID"
```

Validate:

```bash
ps -p "$NOHUP_PID"
```

---

# Phase 7: Investigate Processes Through /proc

## 25. Start a Process for Investigation

Run:

```bash
cd /opt/process-lab
./report-generator.sh &
```

Save its PID:

```bash
PROC_PID=$!
echo "$PROC_PID"
```

---

## 26. Inspect the Process Status

Run:

```bash
cat /proc/"$PROC_PID"/status
```

Find selected fields:

```bash
grep -E '^(Name|State|Pid|PPid|Uid|Gid|Threads|VmSize|VmRSS):' /proc/"$PROC_PID"/status
```

---

## 27. Inspect the Command Line

Run:

```bash
tr '\0' ' ' < /proc/"$PROC_PID"/cmdline
echo
```

---

## 28. Inspect Open File Descriptors

Run:

```bash
ls -l /proc/"$PROC_PID"/fd
```

Count the file descriptors:

```bash
ls /proc/"$PROC_PID"/fd | wc -l
```

File descriptors commonly include:

- `0` — standard input
- `1` — standard output
- `2` — standard error

---

## 29. Inspect Process Limits

Run:

```bash
cat /proc/"$PROC_PID"/limits
```

Look for:

- Maximum open files
- Maximum processes
- Maximum file size
- Maximum locked memory

---

## 30. Inspect the Process Environment

Run:

```bash
tr '\0' '\n' < /proc/"$PROC_PID"/environ | head -20
```

Some systems may restrict access to another user's process environment.

---

## 31. Inspect the Executable and Current Directory

Run:

```bash
readlink -f /proc/"$PROC_PID"/exe
readlink -f /proc/"$PROC_PID"/cwd
```

Because the script is interpreted by Bash, `/proc/<PID>/exe` may point to the Bash executable.

### Validation Checkpoint 7

Record:

| Item | Result |
|---|---|
| PID | |
| PPID | |
| Process state | |
| User ID | |
| Group ID | |
| Resident memory | |
| Number of threads | |
| Number of open file descriptors | |
| Current working directory | |
| Executable interpreter | |

Stop the process:

```bash
kill -15 "$PROC_PID"
```

---

# Phase 8: Parent and Child Processes

## 32. Create a Parent-Child Script

Create:

```bash
vi /opt/process-lab/parent-child.sh
```

Add:

```bash
#!/bin/bash

echo "Parent process started: PID=$$ PPID=$PPID"

sleep 300 &
CHILD1=$!

sleep 300 &
CHILD2=$!

echo "Child 1 PID: $CHILD1"
echo "Child 2 PID: $CHILD2"

wait
```

Make it executable:

```bash
chmod +x /opt/process-lab/parent-child.sh
```

Run it:

```bash
/opt/process-lab/parent-child.sh &
```

Save its PID:

```bash
PARENT_PID=$!
echo "$PARENT_PID"
```

---

## 33. Display the Process Tree

Run:

```bash
pstree -p "$PARENT_PID"
```

Also run:

```bash
ps --forest -o user,pid,ppid,stat,cmd -g "$(ps -o sid= -p "$PARENT_PID" | tr -d ' ')"
```

A simpler alternative is:

```bash
ps -eo user,pid,ppid,stat,cmd --forest | grep -A 5 -B 2 "$PARENT_PID"
```

Show the direct child processes:

```bash
pgrep -P "$PARENT_PID" -a
```

### Validation Checkpoint 8

Confirm that:

- `parent-child.sh` has its own PID.
- Two `sleep` processes have the script PID as their PPID.
- `pstree` displays the hierarchy.

Terminate the parent and children:

```bash
pkill -TERM -P "$PARENT_PID"
kill -TERM "$PARENT_PID"
```

Validate:

```bash
ps -p "$PARENT_PID"
pgrep -P "$PARENT_PID" -a
```

---

# Phase 9: Create and Identify a Zombie Process

## 34. Understand a Zombie Process

A zombie is a process that has finished execution, but its parent has not yet collected its exit status.

A zombie:

- Is already terminated
- Does not consume CPU
- Retains a process table entry
- Usually displays the state `Z`
- Must be handled through its parent process

You normally cannot remove a zombie by killing the zombie itself because it is already dead.

---

## 35. Create the Zombie Program

Create:

```bash
vi /opt/process-lab/zombie.c
```

Add:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(void)
{
    pid_t pid = fork();

    if (pid > 0) {
        printf("Parent PID: %d\n", getpid());
        printf("Child PID: %d\n", pid);
        fflush(stdout);

        sleep(120);
    } else if (pid == 0) {
        printf("Child process %d is exiting now\n", getpid());
        fflush(stdout);
        exit(0);
    } else {
        perror("fork");
        return 1;
    }

    return 0;
}
```

Compile:

```bash
cd /opt/process-lab
gcc zombie.c -o zombie
```

Validate the executable:

```bash
ls -l /opt/process-lab/zombie
file /opt/process-lab/zombie
```

---

## 36. Run and Locate the Zombie

Run:

```bash
cd /opt/process-lab
./zombie &
```

Save the parent PID:

```bash
ZOMBIE_PARENT=$!
echo "$ZOMBIE_PARENT"
```

Wait two seconds:

```bash
sleep 2
```

Display the parent and child:

```bash
ps -o pid,ppid,stat,cmd --ppid "$ZOMBIE_PARENT"
```

Search for all zombie processes:

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /^Z/ {print}'
```

You may see a command similar to:

```text
[zombie] <defunct>
```

Display the process tree:

```bash
pstree -p "$ZOMBIE_PARENT"
```

### Validation Checkpoint 9

Confirm that the child process state includes:

```text
Z
```

Record:

- Zombie PID
- Parent PID
- Process state
- Command name

---

## 37. Remove the Zombie

Terminate the parent:

```bash
kill -15 "$ZOMBIE_PARENT"
```

Wait:

```bash
sleep 2
```

Check again:

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /^Z/ {print}'
```

The zombie created by the lab should no longer appear.

---

# Phase 10: Create a systemd Service

## 38. Prepare the Report Generator for systemd

Ensure the script exists:

```bash
ls -l /opt/process-lab/report-generator.sh
```

Test it manually:

```bash
timeout 5 /opt/process-lab/report-generator.sh
```

A timeout exit status is expected because the script is designed to run continuously.

Check the status code:

```bash
echo $?
```

A common result is:

```text
124
```

This indicates that `timeout` stopped the command.

---

## 39. Create the Service Account

Create a dedicated system account:

```bash
useradd --system \
  --home-dir /opt/process-lab \
  --shell /sbin/nologin \
  reportsvc
```

If the user already exists, confirm it:

```bash
id reportsvc
```

Give the service account access to the project files:

```bash
chown -R reportsvc:reportsvc /opt/process-lab
```

Verify:

```bash
ls -ld /opt/process-lab
ls -l /opt/process-lab/report-generator.sh
```

---

## 40. Improve the Application for Graceful Shutdown

Replace the report generator with a signal-aware version:

```bash
tee /opt/process-lab/report-generator.sh > /dev/null <<'EOF'
#!/bin/bash

LOG_FILE="/tmp/report-generator.log"

cleanup()
{
    echo "$(date): Report Generator PID $$ received a termination signal" >> "$LOG_FILE"
    echo "$(date): Report Generator PID $$ stopped cleanly" >> "$LOG_FILE"
    exit 0
}

trap cleanup SIGTERM SIGINT

echo "$(date): Report Generator started with PID $$ and PPID $PPID" >> "$LOG_FILE"

while true
do
    echo "$(date): PID $$ is generating a report" >> "$LOG_FILE"
    sleep 10 &
    wait $!
done
EOF
```

Set ownership and permissions:

```bash
chown reportsvc:reportsvc /opt/process-lab/report-generator.sh
chmod 755 /opt/process-lab/report-generator.sh
```

Check syntax:

```bash
bash -n /opt/process-lab/report-generator.sh
```

---

## 41. Create the systemd Unit File

Create:

```bash
vi /etc/systemd/system/report-generator.service
```

Add:

```ini
[Unit]
Description=NIT Report Generator
After=network.target

[Service]
Type=simple
User=reportsvc
Group=reportsvc
ExecStart=/opt/process-lab/report-generator.sh
Restart=on-failure
RestartSec=5
TimeoutStopSec=15

[Install]
WantedBy=multi-user.target
```

Validate the unit file:

```bash
systemd-analyze verify /etc/systemd/system/report-generator.service
```

No output normally means that no unit-file error was detected.

---

## 42. Reload, Enable, and Start the Service

Reload systemd:

```bash
systemctl daemon-reload
```

Enable and start the service:

```bash
systemctl enable --now report-generator.service
```

Check status:

```bash
systemctl status report-generator.service --no-pager
```

Check whether it is active:

```bash
systemctl is-active report-generator.service
```

Expected output:

```text
active
```

Check whether it is enabled:

```bash
systemctl is-enabled report-generator.service
```

Expected output:

```text
enabled
```

---

## 43. Locate the Service Process

Display the main PID:

```bash
systemctl show report-generator.service --property=MainPID
```

Save it:

```bash
SERVICE_PID=$(systemctl show report-generator.service --property=MainPID --value)
echo "$SERVICE_PID"
```

Inspect:

```bash
ps -o user,pid,ppid,stat,etime,cmd -p "$SERVICE_PID"
```

The process should run as:

```text
reportsvc
```

Display its hierarchy:

```bash
systemctl status report-generator.service --no-pager
pstree -p "$SERVICE_PID"
```

---

## 44. Test Service Management

Restart:

```bash
systemctl restart report-generator.service
```

Validate:

```bash
systemctl is-active report-generator.service
```

Stop:

```bash
systemctl stop report-generator.service
```

Validate:

```bash
systemctl is-active report-generator.service
```

Expected result:

```text
inactive
```

Start it again:

```bash
systemctl start report-generator.service
```

---

## 45. View Service Logs

The script writes to:

```text
/tmp/report-generator.log
```

View:

```bash
tail -n 20 /tmp/report-generator.log
```

View systemd journal messages:

```bash
journalctl -u report-generator.service --no-pager
```

View the latest messages:

```bash
journalctl -u report-generator.service -n 20 --no-pager
```

Follow the journal in real time:

```bash
journalctl -u report-generator.service -f
```

Press `Ctrl+C` to stop following.

### Validation Checkpoint 10

Confirm that:

- The service starts successfully.
- It is enabled at boot.
- It runs as `reportsvc`.
- It has a MainPID.
- Restarting it gives it a new PID.
- Stopping it writes a graceful shutdown message to the log.

---

# Phase 11: Test Automatic Restart

## 46. Force the Service Process to Fail

Make sure the service is active:

```bash
systemctl start report-generator.service
```

Get its PID:

```bash
SERVICE_PID=$(systemctl show report-generator.service --property=MainPID --value)
echo "$SERVICE_PID"
```

Forcefully terminate it:

```bash
kill -9 "$SERVICE_PID"
```

Wait longer than `RestartSec=5`:

```bash
sleep 7
```

Check the service:

```bash
systemctl status report-generator.service --no-pager
```

Get the new PID:

```bash
NEW_SERVICE_PID=$(systemctl show report-generator.service --property=MainPID --value)
echo "$NEW_SERVICE_PID"
```

Compare:

```bash
echo "Old PID: $SERVICE_PID"
echo "New PID: $NEW_SERVICE_PID"
```

### Validation Checkpoint 11

The new PID should be different from the old PID.

The service should return to:

```text
active
```

This demonstrates the effect of:

```ini
Restart=on-failure
```

---

# Phase 12: Final Troubleshooting Challenge

## 47. Incident Scenario

You are the Linux administrator for NIT Academy.

Users report that:

- The server is slow.
- The report-generator service may not be working correctly.
- A process may be using excessive CPU.
- The service must be restored.
- Evidence must be collected before and after remediation.

Your task is to identify and resolve the incident.

---

## 48. Instructor Preparation

The instructor, or a student completing the self-guided lab, starts the CPU problem.

Because the shell is already running as `root`, use `runuser` to launch the simulated workload as the service account:

```bash
runuser -u reportsvc -- nohup /opt/process-lab/cpu-worker.sh \
  > /tmp/cpu-worker.out 2>&1 &
```

Record the CPU worker PID:

```bash
pgrep -af cpu-worker
```

Optionally stop the report service to create a second issue:

```bash
systemctl stop report-generator.service
```

---

## 49. Student Investigation Procedure

### Step 1: Check system load

```bash
uptime
```

Record:

- Current time
- System uptime
- Number of logged-in users
- Load averages

### Step 2: Identify high CPU usage

```bash
top
```

Or:

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,ni,etime,cmd --sort=-%cpu | head -10
```

### Step 3: Identify the process by name

```bash
pgrep -af cpu-worker
```

### Step 4: Inspect its details

```bash
CPU_INCIDENT_PID=$(pgrep -f '/opt/process-lab/cpu-worker.sh' | head -1)
ps -o user,pid,ppid,stat,%cpu,%mem,ni,etime,cmd -p "$CPU_INCIDENT_PID"
```

### Step 5: Lower its priority

```bash
renice 15 -p "$CPU_INCIDENT_PID"
```

Validate:

```bash
ps -o pid,ni,pri,%cpu,cmd -p "$CPU_INCIDENT_PID"
```

### Step 6: Terminate it gracefully

```bash
kill -15 "$CPU_INCIDENT_PID"
```

Wait:

```bash
sleep 2
```

Validate:

```bash
ps -p "$CPU_INCIDENT_PID"
```

If it remains running:

```bash
kill -9 "$CPU_INCIDENT_PID"
```

### Step 7: Check the application service

```bash
systemctl status report-generator.service --no-pager
```

### Step 8: Review logs

```bash
journalctl -u report-generator.service -n 30 --no-pager
tail -n 20 /tmp/report-generator.log
```

### Step 9: Restore the service

```bash
systemctl restart report-generator.service
```

### Step 10: Validate the recovery

```bash
systemctl is-active report-generator.service
systemctl is-enabled report-generator.service
```

Check its PID:

```bash
systemctl show report-generator.service --property=MainPID
```

Check the process:

```bash
pgrep -af report-generator
```

Check that logs are being generated:

```bash
tail -n 5 /tmp/report-generator.log
sleep 12
tail -n 5 /tmp/report-generator.log
```

Check that the CPU worker is gone:

```bash
pgrep -af cpu-worker
```

Check load again:

```bash
uptime
```

---

## 50. Final Incident Validation

The incident is resolved only when all the following are true:

- The CPU worker is no longer running.
- The report-generator service is active.
- The report-generator service is enabled.
- The service has a valid MainPID.
- The service process is owned by `reportsvc`.
- New report log entries are being created.
- The system load begins to decrease.
- The investigation commands and results are documented.

---

# Phase 13: Automated Validation Script

## 51. Create the Validation Script

Create:

```bash
vi /opt/process-lab/validate-project.sh
```

Add:

```bash
#!/bin/bash

PASS=0
FAIL=0

pass()
{
    echo "[PASS] $1"
    PASS=$((PASS + 1))
}

fail()
{
    echo "[FAIL] $1"
    FAIL=$((FAIL + 1))
}

echo "============================================"
echo " Linux Process Project Validation"
echo "============================================"

if [ -d /opt/process-lab ]; then
    pass "Project directory exists"
else
    fail "Project directory does not exist"
fi

if [ -x /opt/process-lab/report-generator.sh ]; then
    pass "Report generator exists and is executable"
else
    fail "Report generator is missing or not executable"
fi

if bash -n /opt/process-lab/report-generator.sh 2>/dev/null; then
    pass "Report generator passed Bash syntax validation"
else
    fail "Report generator has a Bash syntax error"
fi

if [ -x /opt/process-lab/cpu-worker.sh ]; then
    pass "CPU worker exists and is executable"
else
    fail "CPU worker is missing or not executable"
fi

if bash -n /opt/process-lab/cpu-worker.sh 2>/dev/null; then
    pass "CPU worker passed Bash syntax validation"
else
    fail "CPU worker has a Bash syntax error"
fi

if [ -x /opt/process-lab/parent-child.sh ]; then
    pass "Parent-child script exists and is executable"
else
    fail "Parent-child script is missing or not executable"
fi

if [ -x /opt/process-lab/zombie ]; then
    pass "Zombie demonstration program is compiled"
else
    fail "Zombie demonstration program is not compiled"
fi

if [ -f /etc/systemd/system/report-generator.service ]; then
    pass "systemd unit file exists"
else
    fail "systemd unit file is missing"
fi

if systemctl is-enabled report-generator.service >/dev/null 2>&1; then
    pass "Report generator service is enabled"
else
    fail "Report generator service is not enabled"
fi

if systemctl is-active report-generator.service >/dev/null 2>&1; then
    pass "Report generator service is active"
else
    fail "Report generator service is not active"
fi

SERVICE_PID=$(systemctl show report-generator.service \
    --property=MainPID --value 2>/dev/null)

if [[ "$SERVICE_PID" =~ ^[0-9]+$ ]] && [ "$SERVICE_PID" -gt 0 ]; then
    pass "Service has a valid MainPID: $SERVICE_PID"
else
    fail "Service does not have a valid MainPID"
fi

if [ -n "$SERVICE_PID" ] && ps -p "$SERVICE_PID" -o user= 2>/dev/null \
    | grep -q '^reportsvc$'; then
    pass "Service runs as reportsvc"
else
    fail "Service is not running as reportsvc"
fi

if [ -f /tmp/report-generator.log ]; then
    pass "Report generator log exists"
else
    fail "Report generator log does not exist"
fi

if pgrep -f '/opt/process-lab/cpu-worker.sh' >/dev/null 2>&1; then
    fail "A CPU worker is still running"
else
    pass "No CPU worker is running"
fi

echo
echo "============================================"
echo "Passed: $PASS"
echo "Failed: $FAIL"
echo "============================================"

if [ "$FAIL" -eq 0 ]; then
    echo "PROJECT VALIDATION SUCCESSFUL"
    exit 0
else
    echo "PROJECT VALIDATION FAILED"
    exit 1
fi
```

Make it executable:

```bash
chmod +x /opt/process-lab/validate-project.sh
```

Run it:

```bash
/opt/process-lab/validate-project.sh
```

### Expected final result

```text
PROJECT VALIDATION SUCCESSFUL
```

If a test fails, review the failed item and correct it before running the validation again.

---

# Phase 14: Student Evidence and Deliverables

## 52. Required Screenshots or Command Output

Each student must provide evidence of the following:

1. The project directory and scripts.
2. The report generator running in the foreground.
3. The report generator running as a background job.
4. Output from `jobs -l`.
5. A process paused with state `T`.
6. A process resumed with `SIGCONT`.
7. A successful `SIGTERM`.
8. A successful `SIGKILL`.
9. The CPU worker identified through `top` or `ps`.
10. A changed nice value using `renice`.
11. A process started with `nice -n 15`.
12. A `nohup` process surviving logout.
13. Selected information from `/proc/<PID>/status`.
14. Open file descriptors from `/proc/<PID>/fd`.
15. A parent-and-child process tree.
16. A zombie process with state `Z`.
17. An active and enabled systemd service.
18. The service MainPID.
19. The automatic service restart test.
20. The successful validation script output.

---

## 53. Student Questions

Answer the following in your submission:

1. What is a PID?
2. What is a PPID?
3. What is the difference between a process and a shell job?
4. What does `Ctrl+C` normally send?
5. What does `Ctrl+Z` do?
6. What is the difference between `SIGTERM` and `SIGKILL`?
7. Why should `SIGTERM` normally be tried first?
8. What process state indicates a stopped process?
9. What process state indicates a zombie process?
10. What is the purpose of `SIGCONT`?
11. What is the purpose of `nice`?
12. What happens when a nice value is increased?
13. What is the difference between `nice` and `renice`?
14. Why is `/proc` called a virtual filesystem?
15. What information is available in `/proc/<PID>/status`?
16. What is a file descriptor?
17. What causes a zombie process?
18. Why can a zombie not normally be removed with `kill -9`?
19. What is the purpose of systemd?
20. What does `Restart=on-failure` do?

---

# Phase 15: Incident Report Template

## 54. Student Incident Report

Create a report using the following structure.

```text
Incident Title:
High CPU Utilization and Report Service Interruption

Student Name:
Date:
Server Name:
Server IP:

1. Incident Summary
Describe what users reported.

2. Initial Symptoms
Document load average, CPU usage, service status, and process information.

3. Investigation Commands
List the commands used during troubleshooting.

4. Root Cause
Explain which process caused the issue and why.

5. Corrective Actions
Explain how process priority was changed, how the process was terminated,
and how the service was restored.

6. Validation
Show evidence that the CPU process stopped, the service became active,
and log entries resumed.

7. Preventive Recommendations
Suggest monitoring, resource limits, alerts, and service restart policies.

8. Final Status
Resolved / Not Resolved
```

---

# Phase 16: Suggested Grading Rubric

## 55. Grading

| Area | Points |
|---|---:|
| Project directory and scripts | 5 |
| Process identification | 10 |
| Foreground and background job management | 10 |
| Signal testing | 15 |
| CPU troubleshooting | 10 |
| Nice and renice testing | 10 |
| nohup logout test | 5 |
| `/proc` investigation | 10 |
| Parent-child process tree | 5 |
| Zombie-process demonstration | 5 |
| systemd service configuration | 10 |
| Automatic restart validation | 5 |
| Incident report | 5 |
| **Total** | **100** |

---

# Phase 17: Cleanup

## 56. Stop Lab Processes

Stop any report-generator scripts started outside systemd:

```bash
pkill -TERM -f '/opt/process-lab/report-generator.sh'
```

Stop CPU workers:

```bash
pkill -TERM -f '/opt/process-lab/cpu-worker.sh'
```

Stop parent-child lab processes:

```bash
pkill -TERM -f '/opt/process-lab/parent-child.sh'
```

Stop zombie demonstration processes:

```bash
pkill -TERM -f '/opt/process-lab/zombie'
```

Check:

```bash
pgrep -af 'report-generator|cpu-worker|parent-child|zombie'
```

Be aware that the validation or `pgrep` command itself may briefly appear in matching output.

---

## 57. Keep or Remove the systemd Service

To keep the completed project, leave the service enabled and running.

To disable and remove it:

```bash
systemctl disable --now report-generator.service
rm -f /etc/systemd/system/report-generator.service
systemctl daemon-reload
systemctl reset-failed
```

Remove the service account if it is no longer needed:

```bash
userdel reportsvc
```

Remove project files:

```bash
rm -rf /opt/process-lab
```

Remove temporary logs:

```bash
rm -f \
  /tmp/report-generator.log \
  /tmp/report-output.log \
  /tmp/nohup-report.pid \
  /tmp/cpu-worker.out
```

---

# Phase 18: Troubleshooting Guide

## 58. Permission Denied When Running a Script

Check permissions:

```bash
ls -l /opt/process-lab/*.sh
```

Add execute permission:

```bash
chmod +x /opt/process-lab/*.sh
```

---

## 59. Command Not Found

Install the required package:

```bash
dnf install -y procps-ng psmisc gcc tree
```

Examples:

- `ps`, `top`, and `pgrep` come from `procps-ng`.
- `pstree` and `killall` come from `psmisc`.
- `gcc` compiles the zombie program.

---

## 60. Service Does Not Start

Check status:

```bash
systemctl status report-generator.service --no-pager -l
```

Check logs:

```bash
journalctl -u report-generator.service -n 50 --no-pager
```

Validate the unit:

```bash
systemd-analyze verify /etc/systemd/system/report-generator.service
```

Check the script:

```bash
bash -n /opt/process-lab/report-generator.sh
```

Check permissions:

```bash
namei -l /opt/process-lab/report-generator.sh
```

Reload systemd after changing the unit file:

```bash
systemctl daemon-reload
systemctl restart report-generator.service
```

---

## 61. PID Variable Is Empty

Find the process manually:

```bash
pgrep -af report-generator
```

Then assign the correct PID:

```bash
REPORT_PID=1234
```

Replace `1234` with the real PID.

---

## 62. Process Already Stopped

A message such as:

```text
No such process
```

usually means that the process has already exited or the PID is incorrect.

Verify:

```bash
ps -p PID
```

---

## 63. Zombie Does Not Appear

Run the program again:

```bash
cd /opt/process-lab
./zombie &
```

Wait briefly:

```bash
sleep 1
```

Search:

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /^Z/ {print}'
```

The parent sleeps for 120 seconds, so complete the observation during that period.

---

## 64. CPU Worker Does Not Show High CPU

Verify it is running:

```bash
pgrep -af cpu-worker
```

Check:

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu | head
```

On a multi-vCPU machine, one busy process may use close to 100 percent of one CPU rather than all total CPU capacity.

---

## 65. Log File Permission Problem

Check:

```bash
ls -l /tmp/report-generator.log
```

For this lab, remove the old file and restart the service:

```bash
rm -f /tmp/report-generator.log
systemctl restart report-generator.service
```

The service should recreate it.

---

# Command Reference

## 66. Frequently Used Process Commands

| Command | Purpose |
|---|---|
| `ps -ef` | Display all processes in full format |
| `ps aux` | Display detailed process information |
| `ps -eo ...` | Display selected process columns |
| `top` | Interactive process and resource monitor |
| `pgrep` | Find PIDs by process name or command |
| `pkill` | Send a signal by process name |
| `pstree` | Display parent-child process relationships |
| `jobs` | Display jobs managed by the current shell |
| `bg` | Resume a stopped job in the background |
| `fg` | Move a job into the foreground |
| `kill` | Send a signal to a PID or job |
| `nice` | Start a process with a chosen nice value |
| `renice` | Change the nice value of a running process |
| `nohup` | Run a command that survives logout |
| `systemctl` | Manage systemd services |
| `journalctl` | View systemd journal logs |
| `uptime` | Display uptime and load averages |
| `/proc/<PID>` | Inspect kernel process information |

---

## 67. Common Process States

| State | Meaning |
|---|---|
| `R` | Running or runnable |
| `S` | Interruptible sleep |
| `D` | Uninterruptible sleep |
| `T` | Stopped or traced |
| `Z` | Zombie |
| `I` | Idle kernel thread |

Additional characters may appear in the `STAT` field, such as:

| Character | Meaning |
|---|---|
| `s` | Session leader |
| `l` | Multi-threaded |
| `+` | Foreground process group |
| `<` | High priority |
| `N` | Low priority |

---

## 68. Common Signals

Display available signals:

```bash
kill -l
```

| Signal | Number | Common use |
|---|---:|---|
| `SIGHUP` | 1 | Hangup or configuration reload |
| `SIGINT` | 2 | Interrupt, commonly from `Ctrl+C` |
| `SIGKILL` | 9 | Force immediate termination |
| `SIGTERM` | 15 | Request graceful termination |
| `SIGCONT` | 18 | Continue a stopped process |
| `SIGSTOP` | 19 | Stop a process |
| `SIGTSTP` | 20 | Terminal stop, commonly from `Ctrl+Z` |

---

# Final Project Completion Checklist

## 69. Checklist

- [ ] Required packages installed
- [ ] `/opt/process-lab` created
- [ ] `report-generator.sh` created
- [ ] Foreground process tested
- [ ] Background job tested
- [ ] `jobs`, `bg`, and `fg` tested
- [ ] `SIGSTOP` tested
- [ ] `SIGCONT` tested
- [ ] `SIGTERM` tested
- [ ] `SIGKILL` tested
- [ ] CPU worker created
- [ ] High CPU usage identified
- [ ] `nice` tested
- [ ] `renice` tested
- [ ] `nohup` tested across logout
- [ ] `/proc` inspected
- [ ] Parent-child process tree created
- [ ] Zombie process created and identified
- [ ] Dedicated service account created
- [ ] systemd service created
- [ ] Service enabled and started
- [ ] Automatic restart tested
- [ ] Final incident challenge completed
- [ ] Validation script passed
- [ ] Incident report completed
- [ ] Lab processes cleaned up

---

## Project Completion Standard

The project is complete when:

```bash
/opt/process-lab/validate-project.sh
```

returns:

```text
PROJECT VALIDATION SUCCESSFUL
```

and the student has submitted all required evidence and the completed incident report.
