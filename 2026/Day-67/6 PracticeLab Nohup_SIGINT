# MODULE 08 – Practice Lab: `SIGINT`, `SIGHUP`, and `nohup`
> **Hands-on Practice Lab – Interrupting Processes and Keeping Jobs Running After Terminal Closure**

---

# 🎯 Lab Objective

In this practice lab, you will learn how to:

- Understand the `SIGINT` signal.
- Understand what happens when you press `Ctrl + C`.
- Understand the `SIGHUP` signal.
- Learn what happens when a controlling terminal closes.
- View process relationships using `pstree`.
- Understand why background jobs may terminate after logout.
- Use `nohup` to keep a process running after terminal disconnection.
- Redirect command output when using `nohup`.

---

# 📖 Introduction

Linux uses **signals** to communicate with running processes.

In this lab, we will focus on two important signals:

- `SIGINT`
- `SIGHUP`

We will also learn about the `nohup` command, which helps a process continue running after its terminal is closed.

---

# Main Signals Covered

| Signal | Common Number | Purpose |
|--------|---------------|---------|
| `SIGINT` | `2` | Interrupt a process, commonly through `Ctrl + C` |
| `SIGHUP` | `1` | Notify a process that its controlling terminal has closed |

> Signal numbers are standard on common Linux systems, but you can always verify them using `kill -l`.

---

# 1. View Signal Names and Numbers

Run:

```bash
kill -l
```

You should see signals including:

```text
1) SIGHUP
2) SIGINT
9) SIGKILL
15) SIGTERM
```

To check a specific signal number:

```bash
kill -l 2
```

Expected Output:

```text
INT
```

Check signal 1:

```bash
kill -l 1
```

Expected Output:

```text
HUP
```

---

# 2. What is `SIGINT`?

`SIGINT` means:

> **Signal Interrupt**

Its common signal number is:

```text
2
```

It is normally sent by the controlling terminal when the user presses:

```text
Ctrl + C
```

The purpose of `SIGINT` is to ask a running process to stop.

---

# `SIGINT` Basic Syntax

Using a PID:

```bash
kill -SIGINT PID
```

Using the signal number:

```bash
kill -2 PID
```

Using a shell Job ID:

```bash
kill -SIGINT %1
```

---

# 3. `Ctrl + C` and `SIGINT`

Suppose you run:

```bash
sleep 500
```

The process runs in the foreground.

Press:

```text
Ctrl + C
```

The terminal sends `SIGINT` to the foreground process group.

The process normally terminates.

---

# `SIGINT` Flow

```text
Foreground Process Running
        │
        ▼
User Presses Ctrl + C
        │
        ▼
Terminal Sends SIGINT
        │
        ▼
Process Handles the Signal
        │
        └── Usually Terminates
```

---

# 🔬 Lab 1 – Interrupt a Foreground Process

Run:

```bash
sleep 300
```

Press:

```text
Ctrl + C
```

The command stops, and the shell prompt returns.

Verify that no matching process remains:

```bash
pgrep -a sleep
```

---

# 4. Send `SIGINT` Manually

Start a process in the background:

```bash
sleep 500 &
```

Display its Job ID and PID:

```bash
jobs -l
```

Example:

```text
[1]+ 2548 Running    sleep 500 &
```

Send `SIGINT` using the PID:

```bash
kill -SIGINT 2548
```

Or:

```bash
kill -2 2548
```

Verify:

```bash
ps -p 2548
```

If the process responds to `SIGINT`, it should no longer appear.

---

# Important Property of `SIGINT`

A process can:

- Catch `SIGINT`
- Handle `SIGINT`
- Ignore `SIGINT`

Therefore, not every application must terminate when it receives `SIGINT`.

However, most normal command-line tools terminate when `Ctrl + C` is pressed.

---

# 5. `SIGINT` vs `SIGTERM`

| Feature | `SIGINT` | `SIGTERM` |
|---------|----------|-----------|
| Signal Number | `2` | `15` |
| Common Source | `Ctrl + C` from terminal | `kill PID` |
| Purpose | Interrupt current work | Request graceful termination |
| Can be handled | Yes | Yes |
| Can be ignored | Yes | Yes |
| Typical Use | Stop an interactive foreground command | Stop a process or service cleanly |

---

# 6. What is `SIGHUP`?

`SIGHUP` means:

> **Signal Hangup**

Its common signal number is:

```text
1
```

Historically, this signal was sent when a physical terminal connection was disconnected.

On modern Linux systems, it is often sent when:

- A terminal closes
- An SSH session disconnects
- A controlling shell exits
- A terminal hangup occurs

---

# `SIGHUP` Basic Syntax

Using a PID:

```bash
kill -SIGHUP PID
```

Using the signal number:

```bash
kill -1 PID
```

Using a Job ID:

```bash
kill -SIGHUP %1
```

---

# 7. Controlling Terminal

A process started from a terminal may be associated with that terminal.

Example:

```text
Terminal
   │
   └── Bash Shell
          │
          └── Background Process
```

If the terminal or shell closes, associated jobs may receive:

```text
SIGHUP
```

Depending on how the process handles `SIGHUP`, it may:

- Terminate
- Reload its configuration
- Ignore the signal
- Continue running

---

# 8. Create a Background Process

Start a long-running background process:

```bash
sleep 1000 &
```

Display its PID:

```bash
jobs -l
```

Example:

```text
[1]+ 2548 Running    sleep 1000 &
```

---

# 9. View the Process Tree with `pstree`

Use:

```bash
pstree -p 2548
```

Depending on the version of `pstree`, this may display the process or its descendants.

To view the shell and its related processes, you can also run:

```bash
pstree -p $$
```

Here:

```text
$$
```

represents the PID of the current shell.

Example Output:

```text
bash(2400)───sleep(2548)
```

This shows that the `sleep` process was started by the Bash shell.

---

# Install `pstree` if Missing

On Rocky Linux 9, `pstree` is provided by the `psmisc` package.

Install it using:

```bash
sudo dnf install -y psmisc
```

Then verify:

```bash
pstree --version
```

---

# 10. What Happens When the Terminal Closes?

If you close the terminal or disconnect the SSH session:

- The controlling shell may exit.
- The shell may send `SIGHUP` to its jobs.
- Some jobs may terminate.
- Some jobs may continue, depending on signal handling and shell behavior.

A normal background process is not always guaranteed to survive terminal closure.

---

# Terminal Closure Flow

```text
Background Job Running
        │
        ▼
Terminal or SSH Session Closes
        │
        ▼
Shell Receives/Processes Hangup
        │
        ▼
SIGHUP May Be Sent to Jobs
        │
        ├── Process Terminates
        │
        └── Process Ignores/Handles SIGHUP and Continues
```

---

# 11. Send `SIGHUP` Manually

Start:

```bash
sleep 1000 &
```

Check its PID:

```bash
jobs -l
```

Suppose the PID is:

```text
2548
```

Send `SIGHUP`:

```bash
kill -SIGHUP 2548
```

Or:

```bash
kill -1 2548
```

Verify:

```bash
ps -p 2548
```

The normal `sleep` process will usually terminate.

---

# 12. `SIGHUP` Has Another Common Use

Many daemons use `SIGHUP` as a request to:

> **Reload configuration without fully restarting the process.**

For example, some services may reload configuration when they receive:

```bash
kill -HUP PID
```

However, this behavior depends entirely on the application.

Do not assume every service reloads on `SIGHUP`.

For systemd-managed services, use the service's documented reload command when available:

```bash
systemctl reload service_name
```

---

# 13. What is `nohup`?

`nohup` means:

> **No Hangup**

It starts a command so that it ignores the `SIGHUP` signal.

This helps the process continue running after:

- Terminal closure
- SSH disconnection
- Shell logout

---

# Basic `nohup` Syntax

```bash
nohup command &
```

Example:

```bash
nohup sleep 1000 &
```

---

# 14. Default `nohup` Output

If standard output is connected to the terminal, `nohup` normally redirects it to:

```text
nohup.out
```

Example:

```bash
nohup ./backup.sh &
```

You may see:

```text
nohup: ignoring input and appending output to 'nohup.out'
```

---

# 🔬 Lab 2 – Run a Process with `nohup`

Run:

```bash
nohup sleep 1000 &
```

Example Output:

```text
[1] 2600
```

Verify:

```bash
ps -p 2600 -o pid,ppid,stat,cmd
```

---

# 15. Recommended `nohup` Syntax with Redirection

For a script or command that produces output, use:

```bash
nohup ./backup.sh > /tmp/backup.log 2>&1 &
```

### Explanation

| Part | Meaning |
|------|---------|
| `nohup` | Ignore `SIGHUP` |
| `./backup.sh` | Command or script |
| `> /tmp/backup.log` | Send standard output to a log file |
| `2>&1` | Send standard error to the same file |
| `&` | Run the command in the background |

---

# Example with a Loop

Run:

```bash
nohup bash -c 'while true; do echo "Process still running at $(date)"; sleep 5; done' > /tmp/nohup-test.log 2>&1 &
```

Display the PID:

```bash
echo $!
```

Here:

```text
$!
```

contains the PID of the most recently started background process.

---

# 16. Verify the `nohup` Process

Check the process:

```bash
ps -p $! -o pid,ppid,stat,cmd
```

Monitor its output:

```bash
tail -f /tmp/nohup-test.log
```

Press:

```text
Ctrl + C
```

to exit `tail`. This does not stop the background process.

---

# 17. Test After Logout or Disconnect

A complete test can be performed as follows:

1. Start the process:

   ```bash
   nohup bash -c 'while true; do date; sleep 5; done' > /tmp/nohup-test.log 2>&1 &
   ```

2. Record its PID:

   ```bash
   echo $!
   ```

3. Log out or close the terminal.

4. Reconnect to the server.

5. Verify the process:

   ```bash
   ps -p PID -o pid,ppid,stat,cmd
   ```

6. Check the output:

   ```bash
   tail /tmp/nohup-test.log
   ```

---

# 18. Stop the `nohup` Process

Use the PID:

```bash
kill -TERM PID
```

Verify:

```bash
ps -p PID
```

If it does not stop after a reasonable wait:

```bash
kill -KILL PID
```

Use `SIGKILL` only as a last resort.

---

# 19. `nohup` vs Normal Background Job

| Feature | `command &` | `nohup command &` |
|---------|-------------|-------------------|
| Runs in background | Yes | Yes |
| Terminal remains available | Yes | Yes |
| Ignores `SIGHUP` | Not necessarily | Yes |
| Likely to survive logout | Not guaranteed | Usually |
| Output handling | Terminal unless redirected | `nohup.out` or redirected file |
| Best use | Short background tasks | Tasks that must survive logout |

---

# 20. `nohup` vs `screen`, `tmux`, and `systemd`

| Tool | Best Use |
|------|----------|
| `nohup` | Simple non-interactive command that must survive logout |
| `screen` | Persistent interactive terminal session |
| `tmux` | Modern persistent terminal multiplexer |
| `systemd` | Production service or long-running managed process |

For production workloads, `systemd` is usually more reliable because it provides:

- Automatic restart
- Logging
- Dependency management
- Startup at boot
- Resource controls
- Service status

---

# 21. Complete Signal Flow

```text
Foreground Process
        │
        ├── Ctrl + C
        │      │
        │      ▼
        │    SIGINT
        │      │
        │      └── Usually Terminates
        │
        └── Terminal Closes
               │
               ▼
             SIGHUP
               │
               ├── Normal Process May Terminate
               │
               └── nohup Process Ignores SIGHUP
                            │
                            ▼
                     Continues Running
```

---

# 🧪 Practice Exercises

## Exercise 1 – Test `SIGINT`

Run:

```bash
sleep 300
```

Press:

```text
Ctrl + C
```

Verify that the command stops.

---

## Exercise 2 – Send `SIGINT` Manually

Start:

```bash
sleep 500 &
```

Get the PID:

```bash
jobs -l
```

Send:

```bash
kill -SIGINT PID
```

Verify:

```bash
ps -p PID
```

---

## Exercise 3 – Test `SIGHUP`

Start:

```bash
sleep 600 &
```

Get the PID:

```bash
jobs -l
```

Send:

```bash
kill -SIGHUP PID
```

Verify:

```bash
ps -p PID
```

---

## Exercise 4 – View the Process Tree

Start:

```bash
sleep 700 &
```

Run:

```bash
pstree -p $$
```

Identify the shell and the `sleep` process.

---

## Exercise 5 – Start a `nohup` Process

```bash
nohup sleep 1000 > /tmp/sleep.log 2>&1 &
```

Display the PID:

```bash
echo $!
```

---

## Exercise 6 – Start a Persistent Logging Loop

```bash
nohup bash -c 'while true; do echo "Still running: $(date)"; sleep 5; done' > /tmp/persistent.log 2>&1 &
```

Monitor:

```bash
tail -f /tmp/persistent.log
```

---

## Exercise 7 – Terminate the Persistent Process

Find it:

```bash
pgrep -af 'Still running'
```

Then terminate it:

```bash
kill -TERM PID
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – `Ctrl + C` Does Not Stop the Process

The application may be handling or ignoring `SIGINT`.

Try graceful termination:

```bash
kill -TERM PID
```

If it remains unresponsive:

```bash
kill -KILL PID
```

---

### Scenario 2 – Background Job Stops After SSH Disconnect

Start it using:

```bash
nohup command > output.log 2>&1 &
```

For interactive work, use:

```text
screen
```

or:

```text
tmux
```

---

### Scenario 3 – Cannot Find the PID

Use:

```bash
jobs -l
```

or:

```bash
pgrep -af command_name
```

or:

```bash
ps aux | grep '[c]ommand_name'
```

---

### Scenario 4 – `nohup.out` Was Created

This is normal when output was not explicitly redirected.

View it:

```bash
tail -f nohup.out
```

To use a custom file:

```bash
nohup command > custom.log 2>&1 &
```

---

### Scenario 5 – Process Still Terminates After Using `nohup`

Possible reasons include:

- The process exited because of its own error.
- The system rebooted.
- The process received another termination signal.
- The application depends on terminal input.
- The OOM Killer terminated it.
- The user or administrator stopped it.

Check:

```bash
ps -p PID
```

Review logs:

```bash
journalctl
```

and the redirected output file.

---

# ⚠️ Important Notes

- `Ctrl + C` sends `SIGINT` to the foreground process group, not necessarily only one process.
- `SIGHUP` does not always mean termination; some applications use it to reload configuration.
- `nohup` protects against `SIGHUP`, but it does not protect against:
  - Reboot
  - `SIGTERM`
  - `SIGKILL`
  - Application crashes
  - Resource exhaustion
- A command that requires keyboard input may not work properly under `nohup`.
- Use `systemd` for production services.

---

# 📌 Quick Revision

| Signal or Command | Number | Purpose |
|-------------------|-------:|---------|
| `SIGINT` | `2` | Interrupt a process |
| `Ctrl + C` | — | Send `SIGINT` to foreground process group |
| `SIGHUP` | `1` | Notify process of terminal hangup |
| `kill -INT PID` | `2` | Send `SIGINT` manually |
| `kill -HUP PID` | `1` | Send `SIGHUP` manually |
| `nohup command &` | — | Run command while ignoring `SIGHUP` |
| `pstree -p $$` | — | Show current shell and child processes |
| `echo $!` | — | Show PID of latest background process |

---

# Common Commands

| Command | Purpose |
|---------|---------|
| `kill -l` | List signals |
| `kill -SIGINT PID` | Interrupt a process |
| `kill -2 PID` | Same as `SIGINT` |
| `kill -SIGHUP PID` | Send a hangup signal |
| `kill -1 PID` | Same as `SIGHUP` |
| `nohup command &` | Start a command protected from `SIGHUP` |
| `nohup command > file 2>&1 &` | Redirect output and survive terminal closure |
| `jobs -l` | Display shell jobs and PIDs |
| `pstree -p $$` | Display the shell process tree |
| `pgrep -af name` | Find a process by name |

---

# 📖 Key Takeaways

- `SIGINT` commonly interrupts a foreground process.
- `Ctrl + C` normally sends `SIGINT`.
- `SIGHUP` is commonly associated with terminal closure or session disconnection.
- Normal background jobs may terminate when the shell exits.
- `nohup` makes a process ignore `SIGHUP`.
- Redirect `nohup` output to a log file for easier monitoring.
- `pstree` helps display Parent and Child process relationships.
- `SIGHUP` may reload configuration for some applications.
- `nohup` is suitable for simple persistent jobs, while production services should normally use `systemd`.

---

# 💡 Remember

> **Think of Linux signals as messages sent to a worker.**
>
> - `SIGINT` means: **“Stop what you are doing.”**
> - `SIGHUP` means: **“Your terminal connection has closed.”**
> - `nohup` means: **“Continue working even if the terminal closes.”**
>
> **Golden Flow:**
>
> ```text
> Ctrl + C
>     │
>     ▼
> SIGINT
>     │
>     └── Process Usually Stops
>
> Terminal Closes
>     │
>     ▼
> SIGHUP
>     │
>     ├── Normal Job May Stop
>     │
>     └── nohup Job Continues
> ```
>
> **Use `nohup`, `screen`, `tmux`, or `systemd` whenever a long-running task must continue after you disconnect.**