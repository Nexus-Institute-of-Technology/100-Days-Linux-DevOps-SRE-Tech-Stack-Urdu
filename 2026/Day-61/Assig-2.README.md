# Linux Practice Lab – Managing and Viewing Service Units (RHCSA Style)

## Objective

After completing this lab, you will be able to:

- List all running service units
- Understand the output of `systemctl list-units`
- View all installed service unit files
- Understand different unit file states
- Check whether a service is active
- Check whether a service is enabled
- View the status of individual services
- List failed services for troubleshooting

---

# Lab Environment

- Operating System: Rocky Linux 9
- User: root (or sudo user)

---

# Scenario

As a Linux System Administrator, one of your daily tasks is monitoring and troubleshooting Linux services.

In this lab, you will learn how to list service units, inspect their status, determine whether they are enabled, and identify failed services using the **systemctl** command.

---

# Task 1 – List All Running Service Units

Display all currently loaded service units.

```bash
systemctl list-units --type=service
```

Example Output

```text
UNIT                             LOAD   ACTIVE   SUB      DESCRIPTION

auditd.service                   loaded active   running  Security Auditing Service

chronyd.service                  loaded active   running  NTP client/server

crond.service                    loaded active   running  Command Scheduler

docker.service                   loaded active   running  Docker Application Container Engine

sshd.service                     loaded active   running  OpenSSH Server
```

---

# Understanding the Output

The output contains several important columns.

| Column | Meaning |
|----------|----------|
| UNIT | Name of the service unit |
| LOAD | Whether the unit file has been successfully loaded into memory |
| ACTIVE | High-level state of the service |
| SUB | Low-level detailed status |
| DESCRIPTION | Short description of the service |

---

# Understanding Each Column

## UNIT

Displays the name of the service.

Example

```
sshd.service
```

---

## LOAD

Example

```
loaded
```

Meaning

systemd successfully found the unit file and loaded it into memory.

---

## ACTIVE

Example

```
active
```

Meaning

This is the high-level state of the service.

Common values include

- active
- inactive
- failed

---

## SUB

Example

```
running
```

This provides more detailed information about the service.

Common values include

- running
- exited
- dead
- failed

---

## DESCRIPTION

Provides a short explanation of what the service does.

Example

```
OpenSSH Server
```

---

# Task 2 – Display Loaded and Active Units

Run

```bash
systemctl
```

Notice that this command displays only units that are currently:

- Loaded
- Active

---

# Task 3 – List All Installed Service Unit Files

Display every installed service unit file.

```bash
systemctl list-unit-files --type=service
```

Example

```text
UNIT FILE                   STATE       PRESET

auditd.service              enabled     enabled

chronyd.service             enabled     enabled

containerd.service          disabled    disabled

crond.service               enabled     enabled

dbus-broker.service         enabled     enabled
```

---

# Understanding the Output

Unlike **list-units**, this command displays all installed service unit files, whether they are currently running or not.

---

# Understanding the Columns

| Column | Meaning |
|----------|----------|
| UNIT FILE | Name of the service unit file |
| STATE | Current state of the unit |
| PRESET | Vendor's recommended default state |

---

# Common Unit States

## enabled

The service starts automatically during boot.

---

## disabled

The service does not start automatically during boot.

It can still be started manually.

---

## static

The service cannot be enabled directly.

It is normally started by another unit.

---

## masked

The service has been completely blocked.

It cannot be started until it is unmasked.

---

# Task 4 – View the Status of a Specific Service

Display the status of sshd.

```bash
systemctl status sshd.service
```

Observe

- Loaded
- Active
- PID
- Memory
- CPU
- Recent Logs

---

# Task 5 – Check if a Service is Active

Run

```bash
systemctl is-active sshd.service
```

Example Output

```text
active
```

Possible Outputs

```
active

inactive

failed
```

---

# Task 6 – Check if a Service is Enabled

Run

```bash
systemctl is-enabled sshd.service
```

Example Output

```text
enabled
```

Possible Outputs

```
enabled

disabled

masked

static
```

---

# Task 7 – Disable a Service (Optional)

Disable the service.

```bash
systemctl disable sshd
```

Verify.

```bash
systemctl is-enabled sshd
```

Expected

```text
disabled
```

Enable it again.

```bash
systemctl enable sshd
```

Verify.

```bash
systemctl is-enabled sshd
```

Expected

```text
enabled
```

> **Note:** Only perform this task on a lab system. Disabling SSH on a production server may prevent remote access after reboot.

---

# Task 8 – List Failed Service Units

Display failed services.

```bash
systemctl --failed --type=service
```

Example

```text
UNIT LOAD ACTIVE SUB DESCRIPTION

0 loaded units listed.
```

If services have failed, they will appear in this list.

---

# Why is This Command Important?

This command is extremely useful during troubleshooting.

When users report that a service is not working, one of the first commands a Linux administrator should run is:

```bash
systemctl --failed --type=service
```

It quickly identifies failed services.

---

# Comparing Important Commands

| Command | Purpose |
|----------|----------|
| systemctl | Show active loaded units |
| systemctl list-units --type=service | List running service units |
| systemctl list-unit-files --type=service | List installed service files |
| systemctl status service | Display detailed service information |
| systemctl is-active service | Check if a service is running |
| systemctl is-enabled service | Check if a service starts during boot |
| systemctl --failed --type=service | List failed services |

---

# RHCSA Challenge 1

List all running services.

```bash
systemctl list-units --type=service
```

Count how many services are currently running.

---

# RHCSA Challenge 2

Find the status of the following services.

- sshd
- firewalld
- chronyd
- crond

Record

- Active
- SUB
- Description

---

# RHCSA Challenge 3

List all installed service unit files.

How many are:

- enabled
- disabled
- static
- masked

---

# RHCSA Challenge 4

Determine whether these services are enabled.

- sshd
- chronyd
- firewalld
- crond

---

# RHCSA Challenge 5

Check whether the following services are active.

- sshd
- chronyd
- firewalld

---

# RHCSA Challenge 6

List all failed service units.

If none are present, explain what the output means.

---

# Knowledge Check

1. Which command lists all running service units?

2. Which command lists all installed service unit files?

3. What is the difference between **list-units** and **list-unit-files**?

4. What does the **LOAD** column indicate?

5. What does the **ACTIVE** column indicate?

6. What does the **SUB** column indicate?

7. What does the **DESCRIPTION** column display?

8. Which command checks whether a service is active?

9. Which command checks whether a service is enabled?

10. Which command lists failed services?

11. What does **enabled** mean?

12. What does **disabled** mean?

13. What does **static** mean?

14. What does **masked** mean?

---

# Summary

After completing this lab you should be able to:

✅ List running services

✅ View installed service unit files

✅ Understand the LOAD, ACTIVE, SUB, and DESCRIPTION columns

✅ Check whether a service is active

✅ Check whether a service is enabled

✅ View detailed service information

✅ Identify failed services for troubleshooting

These commands are essential Linux administration skills and are commonly used in RHCSA exams and real-world Linux server administration.