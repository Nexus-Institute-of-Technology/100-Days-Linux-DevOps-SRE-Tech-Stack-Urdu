# MODULE 08 – Practice Lab - OpenSSH Server Configuration
> **Configuring and Securing the OpenSSH Server on Linux**

---

# 🎯 Learning Objectives

In this module, you will learn:

- What the OpenSSH server service is.
- Which daemon provides the SSH service.
- Where the SSH server configuration file is located.
- Why the default OpenSSH configuration may need to be hardened.
- Why direct root login should normally be restricted.
- Why password authentication may be disabled.
- How SSH key-based authentication improves security.
- How to improve accountability by requiring normal user logins.
- How the `PermitRootLogin` setting works.
- How to validate SSH configuration before restarting the service.

---

# 📖 Introduction

OpenSSH provides secure and encrypted remote access to Linux systems.

The SSH server component is provided by the daemon:

```text
sshd
```

The `sshd` service listens for incoming SSH connections, authenticates users, and creates secure remote sessions.

The default OpenSSH server configuration works well for many environments. However, administrators often modify it to improve security.

Common hardening changes include:

- Restricting direct root login.
- Disabling password authentication.
- Requiring SSH key-based authentication.
- Allowing only specific users or groups.
- Changing timeout and authentication settings.
- Reviewing SSH logs and failed login attempts.

---

# 1. What Is the SSH Daemon?

The SSH server daemon is:

```text
sshd
```

It provides the OpenSSH server service.

The daemon is responsible for:

- Listening for incoming SSH connections.
- Verifying the server Host Key.
- Authenticating users.
- Accepting passwords or SSH keys.
- Creating encrypted sessions.
- Starting the remote user's shell.
- Recording authentication events.

---

# 2. OpenSSH Server Configuration File

The main SSH server configuration file is:

```text
/etc/ssh/sshd_config
```

This file controls how the SSH server behaves.

Examples of settings stored in this file include:

- SSH listening port.
- Root login policy.
- Password authentication.
- Public-key authentication.
- Allowed users and groups.
- Login timeouts.
- Maximum authentication attempts.
- Forwarding options.

---

# SSH Client vs SSH Server Configuration

Do not confuse the server and client configuration files.

| File | Purpose |
|------|---------|
| `/etc/ssh/sshd_config` | SSH server configuration |
| `/etc/ssh/ssh_config` | System-wide SSH client configuration |
| `~/.ssh/config` | Per-user SSH client configuration |

The letter `d` in:

```text
sshd_config
```

stands for the SSH daemon.

---

# 3. Check the OpenSSH Server Package

On Rocky Linux, RHEL, or AlmaLinux:

```bash
rpm -q openssh-server
```

If it is not installed:

```bash
sudo dnf install -y openssh-server
```

---

# 4. Check the SSH Service

Check the current status:

```bash
systemctl status sshd
```

Check whether it is active:

```bash
systemctl is-active sshd
```

Check whether it starts automatically at boot:

```bash
systemctl is-enabled sshd
```

---

# Start and Enable the Service

Start the service:

```bash
sudo systemctl start sshd
```

Enable it at boot:

```bash
sudo systemctl enable sshd
```

Enable and start it immediately:

```bash
sudo systemctl enable --now sshd
```

---

# 5. View the Current SSH Configuration

Display the configuration file:

```bash
sudo cat /etc/ssh/sshd_config
```

View it page by page:

```bash
sudo less /etc/ssh/sshd_config
```

Edit it:

```bash
sudo vim /etc/ssh/sshd_config
```

---

# 6. Default Configuration and Security Hardening

The default SSH server configuration is designed to support a wide range of systems.

However, production environments may require stronger controls.

Common changes include:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

These settings:

- Prevent direct root login.
- Disable remote password authentication.
- Allow SSH key-based authentication.

---

# 7. Why Direct Root Login Is Risky

Direct remote login as `root` is generally not recommended.

Some important risks include:

- The username `root` is known on every Linux system.
- Attackers only need to guess the password.
- Root has unrestricted privileges.
- A compromised root account can damage the entire system.
- Multiple administrators may share the same root identity.
- Auditing becomes more difficult.
- Accountability is reduced.

---

# Root Username Is Predictable

Every standard Linux system has a superuser account named:

```text
root
```

An attacker does not need to discover the username.

The attacker can focus only on guessing:

```text
root password
```

This reduces the number of unknown values during a brute-force attack.

---

# Root Has Unlimited Privileges

The `root` user can:

- Modify any file.
- Delete system data.
- Change user passwords.
- Stop services.
- Install software.
- Change security settings.
- Access sensitive information.
- Shut down or reboot the system.

If the root account is compromised, the attacker may gain complete control.

---

# 8. Accountability and Auditing Problems

Suppose ten Linux administrators all use the same root account.

The logs may show:

```text
root performed an action
```

However, it may be difficult to determine which administrator actually performed it.

This creates an accountability problem.

---

# Better Administrative Workflow

Each administrator should:

1. Log in using a personal normal account.
2. Use `sudo` when elevated privileges are required.
3. Allow logs to record which user performed the action.

Example:

```text
nadeem logs in
        │
        ▼
nadeem uses sudo
        │
        ▼
Command runs with root privileges
        │
        ▼
Logs identify nadeem
```

---

# 9. Use `sudo` Instead of Shared Root Login

Example:

```bash
ssh admin1@server
```

Then:

```bash
sudo systemctl restart httpd
```

The system logs can identify:

- The original user.
- The command executed.
- The time of execution.
- Whether the command succeeded.

---

# Check `sudo` Logs

On Rocky Linux or RHEL:

```bash
sudo grep sudo /var/log/secure
```

Using the journal:

```bash
sudo journalctl _COMM=sudo
```

---

# 10. The `PermitRootLogin` Setting

The `PermitRootLogin` setting controls whether the root account can log in through SSH.

It is configured in:

```text
/etc/ssh/sshd_config
```

Example:

```text
PermitRootLogin no
```

---

# Common `PermitRootLogin` Values

| Value | Behavior |
|------|----------|
| `yes` | Root can log in using allowed authentication methods |
| `no` | Root SSH login is completely disabled |
| `prohibit-password` | Root password login is disabled, but key-based login may be allowed |
| `forced-commands-only` | Root login is allowed only for restricted forced commands |

---

# Recommended Settings

Most secure:

```text
PermitRootLogin no
```

For environments that require root key-based automation:

```text
PermitRootLogin prohibit-password
```

The better general approach is:

```text
Normal user login
        │
        ▼
sudo
        │
        ▼
Administrative command
```

---

# 11. Check the Effective Root Login Setting

Run:

```bash
sudo sshd -T | grep -i permitrootlogin
```

Example Output:

```text
permitrootlogin prohibit-password
```

The `sshd -T` command displays the effective SSH server configuration.

This is more reliable than checking only uncommented lines in the file.

---

# 12. Disable Direct Root SSH Login

Edit:

```bash
sudo vim /etc/ssh/sshd_config
```

Set:

```text
PermitRootLogin no
```

Save the file.

Validate the configuration:

```bash
sudo sshd -t
```

If no output appears, the syntax is generally valid.

Restart SSH:

```bash
sudo systemctl restart sshd
```

---

# ⚠️ Avoid Locking Yourself Out

Before disabling root login:

- Create a normal administrative user.
- Add the user to the `wheel` group.
- Test SSH login using that user.
- Confirm that `sudo` works.
- Keep the current root session open during testing.

---

# Create an Administrative User

Example:

```bash
sudo useradd admin1
sudo passwd admin1
sudo usermod -aG wheel admin1
```

Verify:

```bash
id admin1
```

Test:

```bash
ssh admin1@server
```

Then:

```bash
sudo whoami
```

Expected Output:

```text
root
```

---

# 13. Why Disable Password Authentication?

Password authentication may be vulnerable to:

- Brute-force attacks.
- Password guessing.
- Credential reuse.
- Weak passwords.
- Password theft.
- Phishing.
- Automated internet scans.

SSH keys are usually stronger than reusable passwords.

---

# Password Authentication Setting

The setting is:

```text
PasswordAuthentication
```

To disable password login:

```text
PasswordAuthentication no
```

---

# 14. Enable Public-Key Authentication

The relevant setting is:

```text
PubkeyAuthentication yes
```

On many systems, this is enabled by default.

Check:

```bash
sudo sshd -T | grep -i pubkeyauthentication
```

Expected Output:

```text
pubkeyauthentication yes
```

---

# 15. Key-Only Authentication Configuration

A common hardened configuration is:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

This means:

- Root cannot log in directly.
- Public-key authentication is allowed.
- Password authentication is disabled.

---

# ⚠️ Test Keys Before Disabling Passwords

Before setting:

```text
PasswordAuthentication no
```

make sure that:

- SSH keys are configured.
- Public keys are in `authorized_keys`.
- File permissions are correct.
- SELinux contexts are correct.
- A second SSH session can log in successfully.

---

# Safe Testing Procedure

1. Keep the current SSH session open.
2. Open another terminal.
3. Test key-based login.
4. Verify `sudo`.
5. Validate SSH configuration.
6. Restart or reload `sshd`.
7. Test another new session.
8. Close the original session only after success.

---

# 16. Validate the SSH Configuration

Always run:

```bash
sudo sshd -t
```

before restarting the service.

No output generally means:

```text
Configuration syntax is valid
```

If there is an error, it may show:

```text
/etc/ssh/sshd_config line 45: Unsupported option
```

Fix the error before restarting.

---

# 17. Restart or Reload SSH

Restart:

```bash
sudo systemctl restart sshd
```

Reload configuration when supported:

```bash
sudo systemctl reload sshd
```

Check status:

```bash
systemctl status sshd
```

---

# Restart vs Reload

| Action | Meaning |
|--------|---------|
| Restart | Stop and start the SSH service |
| Reload | Re-read configuration without a full restart |

A reload is usually less disruptive, but use the method supported by the service and your environment.

---

# 18. View Effective SSH Settings

Run:

```bash
sudo sshd -T
```

Search for specific settings:

```bash
sudo sshd -T | grep -Ei 'permitrootlogin|passwordauthentication|pubkeyauthentication'
```

Example Output:

```text
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
```

---

# 19. Important SSH Server Settings

| Setting | Purpose |
|---------|---------|
| `Port` | SSH listening port |
| `PermitRootLogin` | Controls remote root login |
| `PasswordAuthentication` | Enables or disables password login |
| `PubkeyAuthentication` | Enables SSH key authentication |
| `MaxAuthTries` | Maximum authentication attempts |
| `LoginGraceTime` | Time allowed to authenticate |
| `AllowUsers` | Allows only named users |
| `AllowGroups` | Allows only members of named groups |
| `DenyUsers` | Blocks named users |
| `DenyGroups` | Blocks named groups |
| `X11Forwarding` | Controls X11 forwarding |
| `AllowTcpForwarding` | Controls SSH port forwarding |
| `ClientAliveInterval` | Checks inactive sessions |
| `ClientAliveCountMax` | Disconnects unresponsive clients |

---

# 20. Change the SSH Port

The standard port is:

```text
22
```

To use a custom port:

```text
Port 2222
```

After changing the port, you must also update:

- Firewall rules.
- SELinux port labeling on enforcing systems.
- Client connection commands.
- Monitoring systems.
- Documentation.

---

# Connect to a Custom Port

```bash
ssh -p 2222 user@server
```

---

# Configure the Firewall for a Custom Port

Example:

```bash
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
```

---

# Configure SELinux for a Custom SSH Port

Install tools if required:

```bash
sudo dnf install -y policycoreutils-python-utils
```

Add the port:

```bash
sudo semanage port -a -t ssh_port_t -p tcp 2222
```

If it already exists under another type, modify it:

```bash
sudo semanage port -m -t ssh_port_t -p tcp 2222
```

Verify:

```bash
sudo semanage port -l | grep ssh
```

---

# Important Note About Changing the Port

Changing the SSH port may reduce automated scanning noise, but it is not a replacement for:

- Strong authentication.
- Firewall restrictions.
- SSH keys.
- Patching.
- Monitoring.
- Rate limiting.

---

# 21. Restrict SSH to Specific Users

Example:

```text
AllowUsers admin1 devops1
```

Only the listed users may connect through SSH.

Check the effective configuration:

```bash
sudo sshd -T | grep -i allowusers
```

---

# Restrict SSH to a Group

Example:

```text
AllowGroups sshusers
```

Create the group:

```bash
sudo groupadd sshusers
```

Add a user:

```bash
sudo usermod -aG sshusers admin1
```

Verify:

```bash
id admin1
```

---

# 22. Control Authentication Attempts

Example:

```text
MaxAuthTries 3
```

This limits the number of authentication attempts per connection.

Check:

```bash
sudo sshd -T | grep -i maxauthtries
```

---

# 23. Configure Login Grace Time

Example:

```text
LoginGraceTime 30
```

This gives users 30 seconds to complete authentication.

Check:

```bash
sudo sshd -T | grep -i logingracetime
```

---

# 24. Disconnect Inactive SSH Sessions

Example:

```text
ClientAliveInterval 300
ClientAliveCountMax 2
```

This means:

- Server checks the client every 300 seconds.
- After two unanswered checks, the session may be disconnected.

---

# 25. Disable Features When Not Required

Examples:

```text
X11Forwarding no
AllowTcpForwarding no
AllowAgentForwarding no
PermitTunnel no
```

Disable features only when your users and applications do not require them.

---

# 26. Use Configuration Drop-In Files

Modern OpenSSH configurations may support:

```text
/etc/ssh/sshd_config.d/
```

Instead of editing the main file directly, create:

```bash
sudo vim /etc/ssh/sshd_config.d/99-hardening.conf
```

Example:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
```

This can make configuration management easier.

---

# Check Include Configuration

The main file may contain:

```text
Include /etc/ssh/sshd_config.d/*.conf
```

Check:

```bash
grep -i '^Include' /etc/ssh/sshd_config
```

---

# 27. Order and Effective Values

SSH configuration may be affected by:

- Main configuration file.
- Drop-in files.
- `Match` blocks.
- Distribution defaults.
- Command-line options.

Always verify the final result with:

```bash
sudo sshd -T
```

---

# 28. Understand `Match` Blocks

A `Match` block applies settings only to certain users, groups, addresses, or hosts.

Example:

```text
Match User backup
    PasswordAuthentication no
    AllowTcpForwarding no
    X11Forwarding no
```

Settings after a `Match` line remain inside that block until another `Match` or the end of the file.

---

# 29. View SSH Authentication Logs

On Rocky Linux or RHEL:

```bash
sudo tail -f /var/log/secure
```

Using the journal:

```bash
sudo journalctl -u sshd
```

Follow in real time:

```bash
sudo journalctl -u sshd -f
```

---

# Common SSH Log Events

Logs may show:

- Successful password login.
- Successful public-key login.
- Failed password.
- Invalid user.
- Root login rejection.
- Disconnection.
- Key authentication failure.
- Configuration errors.

---

# 30. Test Root Login Restriction

From another system:

```bash
ssh root@server
```

If root login is disabled, you may see:

```text
Permission denied
```

Check logs:

```bash
sudo journalctl -u sshd -n 50
```

---

# 31. Test Password Authentication Restriction

Force password authentication:

```bash
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no user@server
```

If password authentication is disabled, the login should fail.

---

# 32. Test Public-Key Authentication

Force public-key authentication:

```bash
ssh -o PreferredAuthentications=publickey user@server
```

For debugging:

```bash
ssh -vv user@server
```

Look for:

```text
Offering public key
Server accepts key
Authenticated using "publickey"
```

---

# 33. Recommended Baseline Configuration

Example hardened configuration:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
X11Forwarding no
AllowAgentForwarding no
```

This is only a sample.

Always confirm application and business requirements before applying it.

---

# 34. Complete Hardening Workflow

```text
Review Current SSH Configuration
              │
              ▼
Create Normal Administrative User
              │
              ▼
Configure SSH Key Authentication
              │
              ▼
Test New Login in a Second Session
              │
              ▼
Disable Direct Root Login
              │
              ▼
Disable Password Authentication
              │
              ▼
Validate with sshd -t
              │
              ▼
Reload or Restart sshd
              │
              ▼
Test Again and Review Logs
```

---

# 🧪 Practice Lab

## Step 1 – Back Up the Configuration

```bash
sudo cp -a /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Verify:

```bash
ls -l /etc/ssh/sshd_config*
```

---

## Step 2 – Check Current Effective Settings

```bash
sudo sshd -T | grep -Ei 'permitrootlogin|passwordauthentication|pubkeyauthentication'
```

---

## Step 3 – Create an Administrative User

```bash
sudo useradd admin1
sudo passwd admin1
sudo usermod -aG wheel admin1
```

---

## Step 4 – Configure SSH Keys for `admin1`

From the client:

```bash
ssh-copy-id admin1@server
```

Test:

```bash
ssh admin1@server
```

---

## Step 5 – Edit the SSH Server Configuration

```bash
sudo vim /etc/ssh/sshd_config
```

Set:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

---

## Step 6 – Validate the Configuration

```bash
sudo sshd -t
```

---

## Step 7 – Restart the Service

```bash
sudo systemctl restart sshd
```

---

## Step 8 – Test in a New Terminal

```bash
ssh admin1@server
```

Then:

```bash
sudo whoami
```

Expected:

```text
root
```

---

## Step 9 – Confirm Root Is Blocked

```bash
ssh root@server
```

---

## Step 10 – Review Logs

```bash
sudo journalctl -u sshd -n 50
```

---

# 🔧 Troubleshooting Scenarios

### Scenario 1 – `sshd` Does Not Restart

Check syntax:

```bash
sudo sshd -t
```

Check status:

```bash
sudo systemctl status sshd
```

Check logs:

```bash
sudo journalctl -u sshd -n 100
```

Restore the backup if necessary:

```bash
sudo cp -a /etc/ssh/sshd_config.backup /etc/ssh/sshd_config
```

---

### Scenario 2 – Key Login Stops Working

Check:

```bash
ssh -vv user@server
```

On the server:

```bash
sudo journalctl -u sshd -n 50
```

Verify:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Restore SELinux contexts:

```bash
sudo restorecon -Rv ~/.ssh
```

---

### Scenario 3 – Root Can Still Log In

Check the effective value:

```bash
sudo sshd -T | grep -i permitrootlogin
```

Search all SSH configuration files:

```bash
sudo grep -Rni 'PermitRootLogin' /etc/ssh/
```

A drop-in file or `Match` block may override the expected configuration.

---

### Scenario 4 – Password Login Still Works

Check:

```bash
sudo sshd -T | grep -i passwordauthentication
```

Search configuration:

```bash
sudo grep -Rni 'PasswordAuthentication' /etc/ssh/
```

Also check:

```text
KbdInteractiveAuthentication
AuthenticationMethods
```

Some systems may allow keyboard-interactive authentication separately.

---

### Scenario 5 – Locked Out After Configuration Change

Use:

- Hypervisor console.
- Physical console.
- Rescue mode.
- Cloud serial console.
- Existing open SSH session.

Restore the previous configuration and restart `sshd`.

---

### Scenario 6 – Custom Port Does Not Work

Check whether SSH is listening:

```bash
sudo ss -tulpn | grep sshd
```

Check firewall:

```bash
sudo firewall-cmd --list-all
```

Check SELinux:

```bash
sudo semanage port -l | grep ssh
```

Test locally:

```bash
ssh -p 2222 localhost
```

---

# ⚠️ Security Best Practices

- Disable direct root login.
- Use personal administrative accounts.
- Use `sudo` for elevated access.
- Prefer SSH keys over passwords.
- Use passphrases on administrator keys.
- Restrict SSH with `AllowUsers` or `AllowGroups`.
- Review SSH logs regularly.
- Keep OpenSSH patched.
- Limit firewall access to trusted networks when possible.
- Use `MaxAuthTries` to reduce repeated attempts.
- Disable forwarding features when they are not required.
- Back up the configuration before making changes.
- Always validate with `sshd -t`.
- Test changes in a second session before closing the first.

---

# 📌 Quick Revision

| Item | Purpose |
|------|---------|
| `sshd` | OpenSSH server daemon |
| `/etc/ssh/sshd_config` | Main SSH server configuration |
| `PermitRootLogin no` | Disable direct root SSH login |
| `PasswordAuthentication no` | Disable SSH password login |
| `PubkeyAuthentication yes` | Enable SSH key authentication |
| `sshd -t` | Validate SSH configuration |
| `sshd -T` | Display effective SSH configuration |
| `systemctl restart sshd` | Restart the SSH service |
| `AllowUsers` | Restrict SSH to specific users |
| `AllowGroups` | Restrict SSH to specific groups |

---

# Common Commands

| Command | Purpose |
|---------|---------|
| `systemctl status sshd` | Check SSH service status |
| `systemctl enable --now sshd` | Enable and start SSH |
| `sudo vim /etc/ssh/sshd_config` | Edit SSH server configuration |
| `sudo sshd -t` | Validate configuration syntax |
| `sudo sshd -T` | Display effective settings |
| `sudo systemctl restart sshd` | Restart SSH |
| `sudo journalctl -u sshd` | View SSH logs |
| `sudo tail -f /var/log/secure` | Follow authentication logs |
| `sudo ss -tulpn | grep sshd` | Check listening SSH ports |
| `ssh -vv user@server` | Debug SSH client authentication |

---

# 📖 Key Takeaways

- The OpenSSH server service is provided by `sshd`.
- The main server configuration file is `/etc/ssh/sshd_config`.
- Direct root login creates security and accountability risks.
- Administrators should log in with personal accounts and use `sudo`.
- SSH keys are generally stronger than password authentication.
- `PermitRootLogin` controls root SSH access.
- `PasswordAuthentication` controls password login.
- `PubkeyAuthentication` controls SSH key login.
- Always run `sshd -t` before restarting the service.
- Always test changes in a second session to avoid lockout.

---

# 💡 Remember

> **Think of the SSH server configuration as the security policy for the remote entrance to your Linux system.**
>
> - `PermitRootLogin` decides whether the superuser may enter directly.
> - `PasswordAuthentication` decides whether passwords are accepted.
> - `PubkeyAuthentication` decides whether SSH keys are accepted.
> - `AllowUsers` and `AllowGroups` decide who may enter.
>
> **Recommended Administrative Flow:**
>
> ```text
> Personal User Account
>          │
>          ▼
> Secure SSH Key Login
>          │
>          ▼
> sudo
>          │
>          ▼
> Administrative Task
> ```
>
> **Never disable your current login method until a safer replacement has been tested successfully.**