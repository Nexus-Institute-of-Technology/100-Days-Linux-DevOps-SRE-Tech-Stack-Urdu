# MODULE 08 – Practice Lab: Linux Process Commands
> **Hands-on Practice Lab – Monitoring Linux Processes with `top` command**

---

# 🎯 Lab Objective

In this practice lab, you will learn how to:

- Use the `top` command to monitor Linux processes.
- Understand the real-time system dashboard.
- Read system uptime and load average.
- Review process states.
- Analyze CPU utilization.
- Check physical memory and swap usage.
- Understand the important columns in the process list.
- Use `top` during Linux troubleshooting.

---

# 📖 Introduction

A Linux System Administrator must know how to monitor running processes and identify system performance problems.

One of the most important commands for this purpose is:

```bash
top
```

The `top` command provides a **dynamic, real-time view** of the Linux system.

It displays:

- System uptime
- Logged-in users
- Load average
- Process states
- CPU utilization
- Memory usage
- Swap usage
- Running processes

The output normally refreshes every few seconds.

---

# 1. Start the `top` Command

Run:

```bash
top
```

The display updates automatically, normally every three seconds.

To exit `top`, press:

```text
q
```

---

# 2. Main Areas of the `top` Display

The default `top` output contains two major sections:

1. **Summary Area**
2. **Process List**

---

## Summary Area

The Summary Area appears at the top of the screen.

It displays:

- Current time
- System uptime
- Logged-in users
- Load average
- Number of tasks
- CPU utilization
- Physical memory usage
- Swap usage

---

## Process List

The Process List appears below the Summary Area.

It displays each running process and information such as:

- PID
- User
- Priority
- Nice value
- Memory usage
- CPU usage
- Process state
- Command name

---

# 3. First Line – Time, Uptime, Users, and Load Average

A typical first line may look similar to:

```text
top - 15:20:41 up 2 days, 3:14, 2 users, load average: 0.20, 0.15, 0.10
```

---

## First-Line Fields

| Field | Meaning |
|------|---------|
| Current time | Current system time |
| `up` | How long the server has been running |
| Users | Number of users currently logged in |
| Load average | Average system workload over 1, 5, and 15 minutes |

---

# Understanding Load Average

Example:

```text
load average: 0.20, 0.15, 0.10
```

The three values represent:

| Value | Period |
|------:|--------|
| `0.20` | Last 1 minute |
| `0.15` | Last 5 minutes |
| `0.10` | Last 15 minutes |

Load average indicates how many tasks are:

- Running on the CPU
- Waiting for CPU time
- Waiting in uninterruptible I/O sleep

---

# Important Note About Load Average

Load average must be interpreted according to the number of logical CPUs.

Examples:

| Logical CPUs | Load Average Near Full Capacity |
|-------------:|---------------------------------:|
| 1 | Around `1.00` |
| 2 | Around `2.00` |
| 4 | Around `4.00` |
| 8 | Around `8.00` |

Therefore, a load average above `2.00` is not automatically a problem on every system.

Check the number of logical CPUs using:

```bash
nproc
```

or:

```bash
lscpu
```

---

# 4. Second Line – Tasks and Process States

A typical second line may look like:

```text
Tasks: 210 total, 1 running, 208 sleeping, 0 stopped, 1 zombie
```

---

## Task-State Fields

| Field | Meaning |
|------|---------|
| Total | Total number of processes or tasks |
| Running | Processes currently running or ready to run |
| Sleeping | Processes waiting for an event or resource |
| Stopped | Suspended processes |
| Zombie | Completed processes waiting for parent cleanup |

---

# Process-State Reminder

| State | Flag | Meaning |
|------|------|---------|
| Running/Runnable | `R` | Running or waiting for CPU |
| Sleeping | `S` | Waiting for an event |
| Uninterruptible Sleep | `D` | Usually waiting for I/O |
| Stopped | `T` | Suspended |
| Zombie | `Z` | Finished but not yet reaped |

---

# 5. Third Line – CPU Usage

A typical CPU line may look like:

```text
%Cpu(s): 2.0 us, 1.0 sy, 0.0 ni, 96.5 id, 0.5 wa, 0.0 hi, 0.0 si, 0.0 st
```

---

# CPU Fields

| Field | Meaning |
|------|---------|
| `us` | CPU time used by user-space processes |
| `sy` | CPU time used by Kernel-space processes |
| `ni` | CPU time used by processes with an adjusted Nice value |
| `id` | Idle CPU time |
| `wa` | CPU time spent waiting for I/O |
| `hi` | Hardware interrupt time |
| `si` | Software interrupt time |
| `st` | CPU time stolen by the hypervisor |

---

# Understanding `us`

`us` represents CPU time spent executing normal user-space programs.

Examples:

- Shell scripts
- Backup scripts
- Web applications
- Database processes
- User commands

A high `us` value may indicate CPU-intensive applications.

---

# Understanding `sy`

`sy` represents CPU time spent inside the Linux Kernel.

Examples:

- System calls
- Device management
- Filesystem operations
- Network processing
- Process scheduling

A consistently high `sy` value may indicate heavy Kernel activity.

---

# Understanding `ni`

`ni` shows CPU time used by processes running with a manually adjusted **Nice value**.

Nice values influence scheduling priority.

---

# Understanding `id`

`id` means CPU idle time.

Example:

```text
100.0 id
```

This means the CPU is completely idle.

A very low `id` value means the CPU is busy.

---

# Understanding `wa`

`wa` means:

> **I/O Wait**

It represents the time during which the CPU is waiting for storage or another I/O operation to complete.

A high `wa` value may indicate:

- Slow disk
- Busy storage
- Heavy backup operation
- Network filesystem delay
- Storage failure
- Database I/O pressure

However, `wa` should not be interpreted alone. Compare it with disk metrics and system activity.

Useful commands include:

```bash
iostat
```

```bash
vmstat
```

```bash
iotop
```

---

# Understanding `hi`, `si`, and `st`

| Field | Meaning |
|------|---------|
| `hi` | Hardware interrupts |
| `si` | Software interrupts |
| `st` | CPU time taken by the hypervisor from a virtual machine |

A high `st` value on a virtual machine may indicate that the hypervisor is overloaded.

---

# 6. Fourth Line – Physical Memory

A typical memory line may look like:

```text
MiB Mem : 7812.0 total, 1200.0 free, 3100.0 used, 3512.0 buff/cache
```

---

## Memory Fields

| Field | Meaning |
|------|---------|
| Total | Total physical RAM |
| Free | Completely unused RAM |
| Used | RAM actively used by processes |
| Buff/Cache | RAM used for buffers and filesystem cache |

---

# Linux Memory Usage

Linux intentionally uses unused RAM for caching.

Therefore, a low `free` value does not always mean the server is running out of memory.

The more useful value is normally:

```text
available
```

It estimates how much memory can be used by new applications without swapping heavily.

---

# 7. Fifth Line – Swap Memory

A typical swap line may look like:

```text
MiB Swap: 2048.0 total, 1900.0 free, 148.0 used, 4200.0 avail Mem
```

---

## Swap Fields

| Field | Meaning |
|------|---------|
| Total | Total configured swap |
| Free | Unused swap |
| Used | Swap currently in use |
| Available Memory | Estimated memory available for new applications |

---

# What is Swap?

Swap is disk space used as an extension of RAM.

If physical memory becomes pressured, Linux may move less-active memory pages to swap.

Some swap usage is not always a problem.

Heavy and continuous swap activity may indicate:

- Insufficient RAM
- Memory leak
- Too many applications
- Improper workload sizing

---

# 8. Process List Columns

Below the Summary Area, `top` displays the Process List.

A typical header looks like:

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
```

---

# Process Column Reference

| Column | Meaning |
|--------|---------|
| `PID` | Process ID |
| `USER` | Owner of the process |
| `PR` | Scheduling priority |
| `NI` | Nice value |
| `VIRT` | Total virtual memory used by the process |
| `RES` | Physical RAM currently used by the process |
| `SHR` | Shared memory used by the process |
| `S` | Process state |
| `%CPU` | Percentage of CPU used |
| `%MEM` | Percentage of physical memory used |
| `TIME+` | Total accumulated CPU time |
| `COMMAND` | Command or process name |

---

# 9. PID Column

`PID` means:

> **Process ID**

Every process has a unique PID.

Example:

```text
PID 1250
```

The PID is used to:

- Track the process
- Send signals
- Change priority
- Terminate the process
- Examine process details

---

# 10. USER Column

The `USER` column shows the owner of the process.

Examples:

```text
root
dev1
apache
mysql
```

The process runs with the permissions and privileges of that user.

---

# 11. PR Column

`PR` means:

> **Priority**

It represents the scheduling priority assigned to the process by the Kernel.

A lower numerical value generally represents a higher scheduling priority.

Some real-time processes may display special values such as:

```text
rt
```

---

# 12. NI Column

`NI` means:

> **Nice Value**

The usual range is:

```text
-20 to 19
```

| Nice Value | Meaning |
|-----------:|---------|
| `-20` | Highest priority |
| `0` | Default priority |
| `19` | Lowest priority |

A lower Nice value gives the process a higher scheduling preference.

---

# 13. VIRT Column

`VIRT` shows the total virtual memory associated with the process.

It may include:

- Process code
- Data
- Shared libraries
- Memory-mapped files
- Allocated but unused memory
- Swapped memory

A high `VIRT` value does not necessarily mean that the process is using the same amount of physical RAM.

---

# 14. RES Column

`RES` means:

> **Resident Memory**

It shows the amount of physical RAM currently occupied by the process.

This value is usually more useful than `VIRT` when investigating actual RAM consumption.

---

# 15. SHR Column

`SHR` shows memory that may be shared with other processes.

Examples include:

- Shared libraries
- Shared memory regions
- Common executable code

---

# 16. S Column

The `S` column shows the current process state.

Examples:

| Value | Meaning |
|------|---------|
| `R` | Running or Runnable |
| `S` | Sleeping |
| `D` | Uninterruptible Sleep |
| `T` | Stopped |
| `Z` | Zombie |
| `I` | Idle Kernel Thread |

---

# 17. `%CPU` Column

`%CPU` shows the share of CPU time used by the process since the last screen refresh.

A high value may indicate a CPU-intensive process.

On a multi-core system, some versions of `top` may show a process using more than `100%` CPU when it uses multiple logical CPUs or threads.

---

# 18. `%MEM` Column

`%MEM` shows the percentage of physical RAM used by the process.

Processes with high `%MEM` should be investigated when the system is under memory pressure.

---

# 19. `TIME+` Column

`TIME+` displays the total CPU time consumed by the process since it started.

This is not the total elapsed clock time.

It represents accumulated CPU execution time.

---

# 20. COMMAND Column

The `COMMAND` column displays:

- Process name
- Program name
- Command line, depending on the display mode

Examples:

```text
sshd
systemd
firewalld
python3
java
```

---

# 🔬 Lab 1 – Run `top`

```bash
top
```

Observe:

- Summary Area
- Process List
- Automatic refresh

Press:

```text
q
```

to exit.

---

# 🔬 Lab 2 – Check Load Average

Run:

```bash
top
```

Look at:

```text
load average
```

Then check CPU count:

```bash
nproc
```

Compare the load with the number of logical CPUs.

---

# 🔬 Lab 3 – Observe Process States

Inside `top`, review the second line:

```text
Tasks:
```

Identify:

- Running
- Sleeping
- Stopped
- Zombie

---

# 🔬 Lab 4 – Check CPU Utilization

Review:

```text
us
sy
ni
id
wa
```

Questions to answer:

- Is the CPU mostly idle?
- Is user-space usage high?
- Is Kernel usage high?
- Is I/O wait high?

---

# 🔬 Lab 5 – Check Memory

Review:

```text
total
free
used
buff/cache
avail Mem
```

Determine whether the system has enough available memory.

---

# 🔬 Lab 6 – Identify Top CPU Consumers

Run:

```bash
top
```

Press:

```text
P
```

This sorts the process list by CPU usage.

Look at:

```text
%CPU
```

---

# 🔬 Lab 7 – Identify Top Memory Consumers

Inside `top`, press:

```text
M
```

This sorts the process list by memory usage.

Look at:

```text
%MEM
```

---

# 🔬 Lab 8 – Search for a Process

Inside `top`, press:

```text
L
```

Enter a search term such as:

```text
sshd
```

Press **Enter**.

---

# 🔬 Lab 9 – View Individual CPU Usage

Inside `top`, press:

```text
1
```

This toggles the display between:

- Combined CPU summary
- Individual logical CPU statistics

---

# 🔬 Lab 10 – Change Refresh Interval

Run:

```bash
top -d 1
```

This refreshes the display every one second.

---

# Useful Interactive `top` Keys

| Key | Action |
|-----|--------|
| `q` | Quit |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `T` | Sort by accumulated CPU time |
| `1` | Show individual CPU statistics |
| `L` | Search for text |
| `k` | Send a signal to a process |
| `r` | Change a process Nice value |
| `h` or `?` | Display help |
| `Space` | Refresh immediately |

---

# ⚠️ Caution with `k` and `r`

Inside `top`:

- `k` can terminate a process.
- `r` can change process scheduling preference.

Use these options carefully, especially on production systems.

---

# Troubleshooting Workflow with `top`

```text
User Reports Slowness
        │
        ▼
Run top
        │
        ├── Check Load Average
        ├── Check CPU Idle
        ├── Check I/O Wait
        ├── Check Available Memory
        ├── Check Swap Usage
        └── Identify Top CPU/Memory Processes
```

---

# Example Troubleshooting Questions

When reviewing `top`, ask:

- Is load average high relative to the CPU count?
- Is `%id` very low?
- Is `%wa` high?
- Is swap heavily used?
- Which process has the highest `%CPU`?
- Which process has the highest `%MEM`?
- Are Zombie processes present?
- Is a process stuck in `D` state?

---

# 🧪 Practice Exercises

## Exercise 1

Start the `top` command.

```bash
top
```

---

## Exercise 2

Record:

- Uptime
- Logged-in users
- 1-minute load average
- 5-minute load average
- 15-minute load average

---

## Exercise 3

Check the number of logical CPUs.

```bash
nproc
```

Compare it with load average.

---

## Exercise 4

Inside `top`, press:

```text
P
```

Identify the process using the most CPU.

---

## Exercise 5

Inside `top`, press:

```text
M
```

Identify the process using the most memory.

---

## Exercise 6

Press:

```text
1
```

Review individual CPU usage.

---

## Exercise 7

Run `top` with a one-second refresh interval.

```bash
top -d 1
```

---

## Exercise 8

Exit `top`.

```text
q
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – System Feels Slow

Run:

```bash
top
```

Check:

- Load average
- `%id`
- `%wa`
- Top CPU processes

---

### Scenario 2 – Suspected Disk Problem

Check the CPU line for:

```text
wa
```

Then investigate further using:

```bash
iostat
```

or:

```bash
vmstat 1
```

---

### Scenario 3 – Memory Pressure

Check:

- Available memory
- Swap used
- Processes with high `%MEM`

Sort by memory:

```text
M
```

---

### Scenario 4 – CPU Saturation

Check:

- Low `%id`
- High load average
- Processes with high `%CPU`

Sort by CPU:

```text
P
```

---

### Scenario 5 – Zombie Processes

Check the Task Summary for:

```text
zombie
```

Then identify them using:

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /^Z/'
```

---

# 📌 Quick Revision

| Item | Meaning |
|------|---------|
| `top` | Real-time process and system monitor |
| Load Average | Workload over 1, 5, and 15 minutes |
| `us` | User-space CPU time |
| `sy` | Kernel-space CPU time |
| `id` | Idle CPU time |
| `wa` | I/O wait |
| `PID` | Process ID |
| `PR` | Process priority |
| `NI` | Nice value |
| `VIRT` | Virtual memory |
| `RES` | Resident physical memory |
| `SHR` | Shared memory |
| `%CPU` | Process CPU usage |
| `%MEM` | Process memory usage |
| `TIME+` | Accumulated CPU time |
| `COMMAND` | Process or command name |

---

# 📖 Key Takeaways

- `top` provides a real-time view of Linux processes and system performance.
- The display contains a Summary Area and a Process List.
- Load average must be compared with the number of logical CPUs.
- High `wa` may indicate storage or I/O pressure.
- Low `id` means the CPU is busy.
- Low free memory alone is not always a problem because Linux uses RAM for cache.
- `RES` shows actual resident physical memory used by a process.
- `%CPU` and `%MEM` help identify resource-intensive processes.
- `P` sorts by CPU and `M` sorts by memory.

---

# 💡 Remember

> **Think of `top` as the live dashboard of a Linux server.**
>
> - Load Average shows how much work is waiting.
> - CPU values show where processor time is being spent.
> - Memory values show how RAM and swap are being used.
> - The Process List shows which applications are consuming resources.
>
> **Basic Troubleshooting Formula:**
>
> ```text
> Check Load
>     │
>     ▼
> Check CPU
>     │
>     ▼
> Check I/O Wait
>     │
>     ▼
> Check Memory and Swap
>     │
>     ▼
> Find the Responsible Process
> ```