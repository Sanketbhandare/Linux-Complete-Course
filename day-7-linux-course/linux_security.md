# Linux Security - OS Hardening | Day 7 | Complete Linux Course

Welcome to Day 7 of the Complete Linux Course. In today's session, we will focus on **Security & OS Hardening** in Linux. This is an essential aspect of maintaining a secure, production-ready Linux system. We'll cover practical examples for hardening a Linux OS, securing user accounts, configuring firewalls, auditing system activities, and much more.

---

## 1. OS Upgrade for Security Patches | (Practical Example)

Keeping your system up-to-date is a vital step in securing your Linux system. In this section, we will go over the practical steps for performing system upgrades and ensuring that security patches are applied correctly.

#### A. Check for Available Updates

Regularly updating your system ensures that all vulnerabilities are patched. Use the following commands to check for available updates:

- On **Debian-based** systems (Ubuntu, Debian):
  ```bash
  sudo apt update            # Update package list
  sudo apt upgrade           # Upgrade all packages
  sudo apt dist-upgrade      # Perform a full system upgrade
  ```

- On **Red Hat-based** systems (CentOS, Fedora, RHEL):
  ```bash
  sudo yum update            # Update packages on CentOS/RHEL
  sudo dnf update            # Update packages on Fedora
  ```

#### B. Automate OS Upgrades

In a production environment, automating OS upgrades helps to ensure that security patches are applied without manual intervention.

- On **Debian/Ubuntu**:
  ```bash
  sudo apt install unattended-upgrades
  sudo dpkg-reconfigure --priority=low unattended-upgrades
  ```

- On **Red Hat-based** systems:
  ```bash
  sudo dnf install dnf-automatic
  sudo systemctl enable --now dnf-automatic.timer
  ```

#### C. Kernel Updates

To ensure your kernel is secure, regularly check for kernel updates and reboot the system after updates.

- To check your current kernel version:
  ```bash
  uname -r
  ```

- To update the kernel on **Debian-based** systems:
  ```bash
  sudo apt-get install linux-image-$(uname -r)
  sudo reboot  # Reboot to apply the kernel update
  ```

---

## 2. Securing User Accounts

Securing user access is one of the primary aspects of hardening a system. Here, we'll discuss methods for enforcing strong password policies, locking inactive accounts, and disabling unnecessary user accounts.

#### A. Enforce Strong Password Policies

**1. /etc/login.defs: To enforce password policies**

Edit the `/etc/login.defs` file to enforce password policies such as minimum length and expiry:
```bash
PASS_MIN_LEN 12        # Minimum password length of 12 characters
PASS_MAX_DAYS 90      # Password must be changed every 90 days
```

**2. /etc/pam.d/common-password: To enforce password complexity**

Enforce password complexity by modifying the `/etc/pam.d/common-password` file:
```bash
password requisite pam_pwquality.so retry=3 minlen=12 minclass=4
```
This configuration ensures passwords are at least 12 characters long and contain at least one uppercase letter, one lowercase letter, one digit, and one special character.

#### B. Lock Inactive Accounts

To prevent unauthorized access through inactive accounts, you can set inactivity limits:
```bash
chage -I 30 -E 2024-12-31 username
```
This command locks the account after 30 days of inactivity and expires it on the given date.

#### C. Disable Unnecessary Accounts

To disable unnecessary or unused accounts:
```bash
passwd -l username
```
This command locks the specified user account.

---

## 3. Managing Sudo Privileges

It's critical to manage **sudo** privileges effectively to prevent unauthorized access to critical system operations.

#### A. Avoid unnecessary sudo access to users

Restrict **sudo** access to only trusted users and limit the commands they can execute.

- Use the `/etc/sudoers` file (edited with `visudo`) to restrict sudo privileges:
  ```bash
  username ALL=(ALL) NOPASSWD: /bin/systemctl restart apache2
  ```

#### B. Use `sudo` logs for auditing

Enable logging for all sudo activities to ensure accountability:
```bash
visudo
Defaults    logfile=/var/log/sudo.log
```

---

## 4. Securing SSH Access

Secure your SSH access to prevent unauthorized logins.

#### A. Disable Root Login via SSH

Prevent direct root login by editing `/etc/ssh/sshd_config`:
```bash
PermitRootLogin no
PasswordAuthentication no
```

- Restart SSH to apply the changes:
  ```bash
  systemctl restart sshd
  ```

#### B. Use SSH Key-Based Authentication

To use SSH keys for authentication:

1. **Generate an SSH key pair**:
   ```bash
   ssh-keygen -t rsa -b 2048
   ```

2. **Copy the public key to the server**:
   ```bash
   ssh-copy-id user@hostname
   ```

#### C. Limit SSH to Specific Users/Groups

In `/etc/ssh/sshd_config`, specify allowed users:
```bash
AllowUsers user1 user2
```

#### D. Change Default SSH Port

Changing the default SSH port helps reduce the risk of brute-force attacks:
```bash
Port 2222
```
- Restart the SSH service:
  ```bash
  systemctl restart sshd
  ```

---

## 5. Firewall Configuration (Using ufw/firewalld)

#### A. Enable and Configure firewalld

**firewalld** is a firewall management tool for Linux systems.

- Start and enable firewalld:
  ```bash
  systemctl start firewalld
  systemctl enable firewalld
  ```

- Allow HTTP and HTTPS traffic:
  ```bash
  firewall-cmd --zone=public --add-service=http --permanent
  firewall-cmd --zone=public --add-service=https --permanent
  ```

- Reload firewalld to apply the changes:
  ```bash
  firewall-cmd --reload
  ```

---

## 6. Disable Unnecessary Services

#### A. List Running Services

List all active services with the following command:
```bash
systemctl list-units --type=service
```

#### B. Disable Unneeded Services

To disable unnecessary services, such as the **nfs** service:
```bash
systemctl disable nfs-server
systemctl stop nfs-server
```

- Disable any other unneeded services:
  ```bash
  systemctl disable <service-name>
  ```

---

## 7. Installing and Configuring SELinux (Security-Enhanced Linux)

**SELinux** enforces security policies on your system by controlling access to resources.

#### A. Check SELinux Status

To check the current status of SELinux:
```bash
sestatus
```

#### B. Enforce SELinux Policy

Ensure SELinux is in **Enforcing** mode:
```bash
setenforce 1
```

To make it permanent, edit the `/etc/selinux/config` file:
```bash
SELINUX=enforcing
```

#### C. Configure SELinux for Services

If you're running a service on a non-default port, use `semanage` to allow it:
```bash
semanage port -a -t http_port_t -p tcp 8080
```

---

## 8. Auditing and Monitoring with auditd

**auditd** is a powerful tool for tracking user activity and file access on your system.

#### A. Install and Configure auditd

Install **auditd**:
```bash
sudo yum install audit
```

- Start and enable auditd:
  ```bash
  systemctl start auditd
  systemctl enable auditd
  ```

#### B. Create Custom Audit Rules

To monitor access to sensitive files, such as `/etc/passwd`:
```bash
-a always,exit -F dir=/etc -F name=passwd -F perm=r
```

- View audit logs:
  ```bash
  ausearch -f /etc/passwd
  ```

---

## 9. Log Management and Rotation

Logs are crucial for security auditing, but they can consume significant disk space if not managed properly.

#### A. Configure Log Rotation

Edit `/etc/logrotate.d/httpd` to configure log rotation for Apache logs:
```bash
/var/log/httpd/*log {
    daily
    rotate 7
    compress
    missingok
    notifempty
    create 640 apache apache
}
```

#### B. Monitor Logs for Suspicious Activity

Use **logwatch** to monitor system logs and send email reports:
```bash
yum install logwatch
logwatch --output mail --mailto admin@example.com --detail high
```

---

## 10. Encryption and Secure Communication

Encrypt sensitive data and ensure secure communication protocols are used.

#### A. Encrypt Sensitive Files

To encrypt sensitive files using **GPG**:
```bash
gpg --encrypt --recipient 'username' filename
```

#### B. Use Secure Communication Protocols

Always use secure communication protocols like **SSH**, **SFTP**, and **HTTPS** to ensure the confidentiality and integrity of data in transit.

---

**By following these steps, you can ensure your Linux system is hardened and secure, ready to run in production environments while minimizing vulnerabilities.**
<hr/>