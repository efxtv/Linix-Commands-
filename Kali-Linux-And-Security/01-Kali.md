# 🛡️Kali Linux 2026 | Complete Security & Setup Guide
### For QEMU/KVM Virtual Machine (qcow2 Image)
> **Author:** Security Hardening Reference Guide (EFXTv)  
> **Version:** Kali Linux 2026 | QEMU/KVM | Updated: July 2026  
> **Audience:** Beginners to Intermediate Linux Users & Ethical Hackers
> Join to learn more on [Telegram](https://t.me/+egpQDeBtGk8wYWU1)

---

> ⚠️ **DISCLAIMER:** This guide is for **educational and ethical use only**. Always practice security in environments you own or have explicit written permission to test. Never use these techniques on unauthorized systems.

---

## 📚 Table of Contents

1. [First Boot Essentials](#1-first-boot-essentials)
2. [QEMU/KVM VM Optimization](#2-qemukvm-vm-optimization)
3. [Firewall Setup — UFW](#3-firewall-setup--ufw)
4. [Brute Force Protection — Fail2Ban](#4-brute-force-protection--fail2ban)
5. [SSH Hardening](#5-ssh-hardening)
6. [Two-Factor Authentication (2FA)](#6-two-factor-authentication-2fa)
7. [User Account Security](#7-user-account-security)
8. [Kernel Hardening — Sysctl](#8-kernel-hardening--sysctl)
9. [Mandatory Access Control — AppArmor](#9-mandatory-access-control--apparmor)
10. [Rootkit Detection — RKHunter & Chkrootkit](#10-rootkit-detection--rkhunter--chkrootkit)
11. [Antivirus — ClamAV](#11-antivirus--clamav)
12. [Intrusion Detection — Snort / Suricata](#12-intrusion-detection--snort--suricata)
13. [System Audit Logging — Auditd](#13-system-audit-logging--auditd)
14. [File Integrity Monitoring — AIDE](#14-file-integrity-monitoring--aide)
15. [Security Audit — Lynis](#15-security-audit--lynis)
16. [Network Monitoring & Anti-Bot](#16-network-monitoring--anti-bot)
17. [Disable Unnecessary Services](#17-disable-unnecessary-services)
18. [Log Monitoring & Management](#18-log-monitoring--management)
19. [Automatic Security Updates](#19-automatic-security-updates)
20. [Encrypt Sensitive Data](#20-encrypt-sensitive-data)
21. [VM Snapshots & Backups](#21-vm-snapshots--backups)
22. [Install Essential Kali Tools](#22-install-essential-kali-tools)
23. [Quality of Life Setup](#23-quality-of-life-setup)
24. [Security Maintenance Checklist](#24-security-maintenance-checklist)
25. [Quick Reference Cheatsheet](#25-quick-reference-cheatsheet)

---

## 1. First Boot Essentials

### 🔑 Default Credentials (CHANGE IMMEDIATELY!)
The official Kali Linux QEMU `.qcow2` image ships with:
- **Username:** `kali`
- **Password:** `kali`

> 🚨 These are public knowledge. Every bot in the world knows this. Change them the second you boot up.

```bash
# Change current user password
passwd

# Change root password
sudo passwd root
```

### 🔄 Update the Entire System
```bash
sudo apt update && sudo apt upgrade -y
sudo apt dist-upgrade -y
sudo apt autoremove -y
sudo apt autoclean
```

> 💡 Always do this before anything else. Unpatched packages are the #1 way systems get compromised.

---

## 2. QEMU/KVM VM Optimization

### 🖥️ Install SPICE Guest Agent (Must-Do for QEMU!)
SPICE agent enables clipboard sharing, auto screen resolution, and file drag & drop between host and VM.

```bash
sudo apt install spice-vdagent -y
sudo systemctl enable spice-vdagentd
sudo reboot
```

### ⚡ Verify KVM Acceleration is Active
```bash
# Inside the VM
dmesg | grep -i kvm

# On your host machine
lsmod | grep kvm
egrep -c '(vmx|svm)' /proc/cpuinfo  # Should return > 0
```

### ⚙️ Virt-Manager VM Settings to Optimize
Go to your VM settings in Virt-Manager and configure:

| Setting | Recommended Value |
|---|---|
| **CPU Model** | `host-passthrough` |
| **Video Model** | `QXL` or `VirtIO` |
| **Display** | `SPICE` (not VNC) |
| **NIC Model** | `VirtIO` |
| **Disk Bus** | `VirtIO` |
| **Cache Mode** | `writeback` |

### 📁 Set Up a Shared Folder (Host ↔ VM)
In Virt-Manager:
1. Go to **VM Settings → Add Hardware → Filesystem**
2. Set **Source Path** (your host folder) and **Target Path**

Inside Kali VM:
```bash
sudo mkdir -p /mnt/shared
sudo mount -t 9p -o trans=virtio /sharedfolder /mnt/shared

# Make it auto-mount on boot
echo "sharedfolder /mnt/shared 9p trans=virtio,version=9p2000.L,rw,_netdev,nofail 0 0" | sudo tee -a /etc/fstab
```

### 📸 Take a Clean Snapshot NOW
Before you do anything else — take a snapshot of this clean state. This is your "golden image" to revert to.

> In Virt-Manager: **View → Snapshots → Take Snapshot** → Name it `"clean-install-baseline"`

---

## 3. Firewall Setup — UFW

UFW (Uncomplicated Firewall) is a user-friendly frontend for `iptables`. It is your first and most important line of defense.

### Install & Configure UFW
```bash
sudo apt install ufw -y
```

### Set Default Policies
```bash
# Deny ALL incoming connections by default
sudo ufw default deny incoming

# Allow all outgoing connections
sudo ufw default allow outgoing

# Deny all forwarding (you're not a router)
sudo ufw default deny forward
```

### Allow Only What You Need
```bash
# Allow SSH (use YOUR port, not 22)
sudo ufw allow 2222/tcp comment 'SSH'

# Allow HTTP/HTTPS only if you run a web server
sudo ufw allow 80/tcp comment 'HTTP'
sudo ufw allow 443/tcp comment 'HTTPS'

# Rate-limit SSH to prevent brute force
sudo ufw limit 2222/tcp comment 'SSH Rate Limit'
```

### Enable UFW
```bash
sudo ufw enable
sudo ufw status verbose
```

### Useful UFW Commands
```bash
# List all rules with numbers
sudo ufw status numbered

# Delete a rule by number
sudo ufw delete 3

# Block a specific IP address
sudo ufw deny from 192.168.1.100

# Block an entire subnet
sudo ufw deny from 10.0.0.0/8

# Allow only a specific IP to connect on SSH
sudo ufw allow from 192.168.1.50 to any port 2222

# Disable UFW temporarily
sudo ufw disable

# Reset ALL rules (start fresh)
sudo ufw reset
```

### Enable UFW Logging
```bash
sudo ufw logging on
sudo ufw logging medium   # Options: low, medium, high, full

# View firewall logs
sudo tail -f /var/log/ufw.log
```

> 💡 **Pro Tip:** For a pentesting VM, keep `deny incoming` as the default. Only open ports when actively running services. Close them when done.

---

## 4. Brute Force Protection — Fail2Ban

Fail2Ban watches your log files and automatically bans IP addresses that show brute-force behaviour — too many failed login attempts, too many port scans, etc. This is your anti-bot shield.

### Install Fail2Ban
```bash
sudo apt install fail2ban -y
```

### Configure Fail2Ban
Always edit the **local** config, never the original:
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

Set these values in `[DEFAULT]`:
```ini
[DEFAULT]
# Ban IP for 24 hours
bantime  = 86400

# Look back 10 minutes for failures
findtime = 600

# Ban after 3 failed attempts
maxretry = 3

# Your own IPs that should NEVER be banned
ignoreip = 127.0.0.1/8 ::1 192.168.1.0/24

# Email alerts (optional, if you have mail setup)
destemail = your@email.com
action = %(action_mwl)s
```

Enable SSH jail:
```ini
[sshd]
enabled  = true
port     = 2222
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 3
bantime  = 86400
```

Enable HTTP protection (if running a web server):
```ini
[apache-auth]
enabled = true

[nginx-http-auth]
enabled = true
```

### Start & Enable Fail2Ban
```bash
sudo systemctl enable fail2ban --now
sudo systemctl status fail2ban
```

### Useful Fail2Ban Commands
```bash
# Check status of all jails
sudo fail2ban-client status

# Check SSH jail status and see banned IPs
sudo fail2ban-client status sshd

# Manually ban an IP
sudo fail2ban-client set sshd banip 1.2.3.4

# Unban an IP
sudo fail2ban-client set sshd unbanip 1.2.3.4

# View real-time fail2ban log
sudo tail -f /var/log/fail2ban.log
```

> 🤖 **Bot Reality Check:** Within minutes of connecting to the internet, bots will start hammering port 22. Fail2Ban + UFW rate limiting together dramatically cuts this noise.

---

## 5. SSH Hardening

SSH is the most commonly attacked service on any Linux system. Harden it properly.

### Install OpenSSH Server
```bash
sudo apt install openssh-server -y
```

### Edit SSH Configuration
```bash
sudo nano /etc/ssh/sshd_config
```

Apply all of these settings:
```ini
# Change default port (bots primarily scan port 22)
Port 2222

# Disable root login completely
PermitRootLogin no

# Force SSH Protocol version 2 only
Protocol 2

# Disable password auth — use keys only (after setting up keys!)
PasswordAuthentication no
PermitEmptyPasswords no

# Limit login attempts
MaxAuthTries 3
MaxSessions 2

# Limit who can login
AllowUsers kali   # Replace with your username

# Timeout idle sessions (5 minutes)
ClientAliveInterval 300
ClientAliveCountMax 2

# Disable insecure/unnecessary features
X11Forwarding no
AllowAgentForwarding no
AllowTcpForwarding no
PermitTunnel no

# Use only strong ciphers
Ciphers aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
KexAlgorithms curve25519-sha256,diffie-hellman-group14-sha256

# Log level for monitoring
LogLevel VERBOSE

# Use PAM
UsePAM yes

# Show last login info
PrintLastLog yes
```

### Generate SSH Key Pair (Key-Based Auth)
```bash
# Generate a strong Ed25519 key
ssh-keygen -t ed25519 -b 4096 -C "kali-vm-$(date +%Y%m%d)"

# Copy public key to authorized_keys
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

### Lock Down SSH Config File Permissions
```bash
sudo chown root:root /etc/ssh/sshd_config
sudo chmod 600 /etc/ssh/sshd_config

# Restart SSH to apply all changes
sudo systemctl restart ssh
sudo systemctl status ssh
```

### Add a Login Banner (Deter Attackers)
```bash
sudo nano /etc/issue.net
```
Add:
```
***************************************************************************
                         AUTHORIZED ACCESS ONLY
  This system is monitored. All activity is logged and reviewed.
  Unauthorized access is a criminal offense under the IT Act.
  Disconnect NOW if you are not an authorized user.
***************************************************************************
```

Then enable the banner in sshd_config:
```ini
Banner /etc/issue.net
```

---

## 6. Two-Factor Authentication (2FA)

Even if an attacker gets your password AND your SSH key, 2FA stops them cold.

### Install Google Authenticator PAM
```bash
sudo apt install libpam-google-authenticator -y
```

### Configure 2FA for Your User
```bash
# Run as your regular user (not root)
google-authenticator
```

Answer the prompts:
- Time-based tokens? → **y**
- Scan the QR code with Google Authenticator / Authy app on your phone
- Update the `.google_authenticator` file? → **y**
- Disallow multiple uses of same token? → **y**
- Increase time window? → **n**
- Enable rate limiting? → **y**

> 🔐 **Save your backup scratch codes in a safe place offline!**

### Configure PAM for SSH
```bash
sudo nano /etc/pam.d/sshd
```

Add this line at the top:
```
auth required pam_google_authenticator.so
```

Comment out the `@include common-auth` line:
```
#@include common-auth
```

### Update sshd_config for 2FA
```bash
sudo nano /etc/ssh/sshd_config
```

Set:
```ini
ChallengeResponseAuthentication yes
KbdInteractiveAuthentication yes
AuthenticationMethods publickey,keyboard-interactive
UsePAM yes
```

```bash
sudo systemctl restart ssh
```

> 🧪 **Test in a new terminal before closing your current session!** If it fails you can still fix it from the current session.

---

## 7. User Account Security

### Lock the Root Account
```bash
# Lock root password (use sudo instead)
sudo passwd -l root

# Verify root is locked
sudo passwd -S root  # Should show 'L' status
```

### Create a Non-Root User for Daily Use
```bash
sudo adduser yourname
sudo usermod -aG sudo yourname

# Switch to new user and test sudo
su - yourname
sudo whoami   # Should return: root
```

### Set Password Quality Policy
```bash
sudo apt install libpam-pwquality -y
sudo nano /etc/security/pwquality.conf
```

Set:
```ini
# Minimum password length
minlen = 12

# Require at least 1 uppercase, 1 digit, 1 special char
ucredit = -1
dcredit = -1
ocredit = -1
lcredit = -1

# Reject passwords similar to username
usercheck = 1

# Reject passwords based on dictionary words
dictcheck = 1
```

### Set Password Expiry Policy
```bash
sudo nano /etc/login.defs
```

```ini
PASS_MAX_DAYS   90    # Expire every 90 days
PASS_MIN_DAYS   10    # Can't change before 10 days
PASS_WARN_AGE    7    # Warn 7 days before expiry
```

Apply to existing user:
```bash
sudo chage -M 90 -m 10 -W 7 kali
sudo chage -l kali   # View settings
```

### Audit User Accounts
```bash
# Check for accounts with empty passwords (DANGER!)
sudo awk -F: '($2 == "") {print "EMPTY PASSWORD: " $1}' /etc/shadow

# Check for accounts with UID 0 (root-level) — should only be root
awk -F: '($3 == 0) {print "ROOT-LEVEL USER: " $1}' /etc/passwd

# List all users with login shells
grep -v '/nologin\|/false' /etc/passwd

# Check who has sudo access
sudo cat /etc/sudoers
sudo grep -r 'sudo\|wheel' /etc/group
```

### Restrict su Command (Only Allow Sudo Group)
```bash
sudo dpkg-statoverride --update --add root sudo 4750 /bin/su
```

---

## 8. Kernel Hardening — Sysctl

The Linux kernel has many network security settings that are off by default. These protect against common network attacks, IP spoofing, SYN floods, and more.

```bash
sudo nano /etc/sysctl.conf
```

Add all of these:
```ini
###############################################
# KALI LINUX — KERNEL SECURITY HARDENING
###############################################

## --- IP FORWARDING ---
# Disable IP forwarding (we're not a router)
net.ipv4.ip_forward = 0
net.ipv6.conf.all.forwarding = 0

## --- SYN FLOOD PROTECTION ---
# Enable SYN cookies to handle SYN flood attacks
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 2048
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries = 5

## --- ICMP PROTECTION ---
# Ignore ICMP ping broadcast requests (Smurf attack protection)
net.ipv4.icmp_echo_ignore_broadcasts = 1

# Ignore bogus ICMP error responses
net.ipv4.icmp_ignore_bogus_error_responses = 1

## --- REDIRECT PROTECTION ---
# Disable accepting ICMP redirects (MITM prevention)
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0

# Disable sending ICMP redirects
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

## --- SOURCE ROUTING ---
# Disable source routing (IP spoofing prevention)
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0

## --- REVERSE PATH FILTERING (Anti-Spoofing) ---
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1

## --- LOG SUSPICIOUS PACKETS ---
# Log martian packets (fake/invalid source addresses)
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1

## --- MEMORY PROTECTION ---
# Restrict kernel pointer exposure
kernel.kptr_restrict = 2

# Restrict dmesg access to root only
kernel.dmesg_restrict = 1

# Protect against ptrace (process injection attacks)
kernel.yama.ptrace_scope = 1

# Disable magic SysRq key (prevents reboots from keyboard)
kernel.sysrq = 0

## --- CORE DUMPS ---
# Disable core dumps (prevent password leaks in crash dumps)
fs.suid_dumpable = 0
kernel.core_uses_pid = 1

## --- RANDOMIZE MEMORY (ASLR) ---
# Full address space layout randomization
kernel.randomize_va_space = 2

## --- TCP HARDENING ---
# Protect against TIME-WAIT assassination
net.ipv4.tcp_rfc1337 = 1

# Disable TCP timestamps (prevents uptime detection)
net.ipv4.tcp_timestamps = 0
```

Apply immediately:
```bash
sudo sysctl -p
sudo sysctl --system
```

---

## 9. Mandatory Access Control — AppArmor

AppArmor confines programs to a limited set of resources. Even if a tool gets exploited, AppArmor prevents it from doing damage outside its allowed scope.

```bash
# Install AppArmor
sudo apt install apparmor apparmor-utils apparmor-profiles apparmor-profiles-extra -y

# Enable AppArmor
sudo systemctl enable apparmor --now

# Check status and see which profiles are loaded
sudo aa-status
```

### Manage AppArmor Profiles
```bash
# Set all profiles to enforce mode (active protection)
sudo aa-enforce /etc/apparmor.d/*

# Check if a specific app has a profile
sudo aa-status | grep firefox
sudo aa-status | grep nginx

# Put a profile in complain mode (logs but doesn't block — for testing)
sudo aa-complain /etc/apparmor.d/usr.bin.firefox

# Put it back to enforce
sudo aa-enforce /etc/apparmor.d/usr.bin.firefox

# Generate a new profile for a program
sudo aa-genprof /usr/bin/yourprogram

# View AppArmor denials in real time
sudo journalctl -f | grep -i apparmor
```

---

## 10. Rootkit Detection — RKHunter & Chkrootkit

Rootkits are malware that hide themselves deep inside the OS. Use BOTH scanners — they catch different things.

### RKHunter
```bash
# Install
sudo apt install rkhunter -y

# Update signature database
sudo rkhunter --update
sudo rkhunter --propupd   # Build baseline of your system files

# Run a full scan
sudo rkhunter --check

# Run scan with no interactive prompts
sudo rkhunter --check --skip-keypress --report-warnings-only

# Automate daily scans at 3 AM
echo "0 3 * * * root /usr/bin/rkhunter --check --skip-keypress --report-warnings-only 2>&1 | mail -s '[Kali] RKHunter Report' root" | sudo tee -a /etc/crontab
```

### Chkrootkit
```bash
# Install
sudo apt install chkrootkit -y

# Run a scan
sudo chkrootkit

# Run and show only infections
sudo chkrootkit | grep -i infected

# Automate weekly scan
echo "0 4 * * 0 root /usr/sbin/chkrootkit 2>&1 | mail -s '[Kali] Chkrootkit Report' root" | sudo tee -a /etc/crontab
```

> ⚠️ **Important:** After installing and configuring your system, always run `sudo rkhunter --propupd` to create a new baseline. Then future scans compare against YOUR known-good state.

---

## 11. Antivirus — ClamAV

ClamAV is an open-source antivirus. Useful for scanning files received from the internet, email attachments, or files you'll be analyzing during pentesting.

```bash
# Install ClamAV
sudo apt install clamav clamav-daemon -y

# Update virus database
sudo freshclam

# Start the ClamAV daemon
sudo systemctl enable clamav-daemon --now

# Scan a specific directory
sudo clamscan -r /home/kali --bell -i

# Scan entire system (slow but thorough)
sudo clamscan -r / --exclude-dir="^/sys|^/proc|^/dev" -i --log=/var/log/clamav-scan.log

# Scan and automatically remove infected files (BE CAREFUL!)
sudo clamscan -r /home/kali --remove=yes -i

# Automate weekly full scan
echo "0 2 * * 6 root /usr/bin/clamscan -r /home --bell -i --log=/var/log/weekly-clamscan.log" | sudo tee -a /etc/crontab
```

---

## 12. Intrusion Detection — Snort / Suricata

These are **Network Intrusion Detection Systems (NIDS)** — they watch your network traffic in real time and alert on suspicious patterns, known attack signatures, port scans, exploits, etc.

### Option A: Suricata (Recommended — More Modern)
```bash
# Install Suricata
sudo apt install suricata -y

# Update rules (uses Emerging Threats ruleset)
sudo suricata-update

# Test configuration
sudo suricata -T -c /etc/suricata/suricata.yaml -v

# Start Suricata
sudo systemctl enable suricata --now

# Monitor alerts in real time
sudo tail -f /var/log/suricata/fast.log

# View detailed alerts (JSON format)
sudo tail -f /var/log/suricata/eve.json | python3 -m json.tool
```

### Option B: Snort (Classic, Battle-Tested)
```bash
sudo apt install snort -y

# Run Snort in IDS mode on your interface
sudo snort -A console -q -u snort -g snort -c /etc/snort/snort.conf -i eth0

# View alerts
sudo tail -f /var/log/snort/alert
```

> 🤖 **Why this matters for bots:** Suricata/Snort will detect port scans, SYN floods, known exploit attempts, and even cryptocurrency mining traffic. It's your network-level bot radar.

---

## 13. System Audit Logging — Auditd

Auditd tracks **who did what** on your system — file reads/writes, commands run as root, login attempts, sudo usage, and more. It creates a forensic trail.

```bash
# Install
sudo apt install auditd audispd-plugins -y
sudo systemctl enable auditd --now
```

### Set Up Audit Rules
```bash
sudo nano /etc/audit/rules.d/hardening.rules
```

Add these rules:
```bash
# Delete existing rules
-D

# Set buffer size
-b 8192

# Monitor critical system files
-w /etc/passwd -p wa -k user_changes
-w /etc/shadow -p wa -k shadow_changes
-w /etc/group -p wa -k group_changes
-w /etc/sudoers -p wa -k sudoers_changes
-w /etc/ssh/sshd_config -p wa -k ssh_config_changes

# Monitor authentication attempts
-w /var/log/auth.log -p wa -k auth_log

# Monitor cron jobs
-w /etc/crontab -p wa -k cron_changes
-w /etc/cron.d/ -p wa -k cron_changes
-w /var/spool/cron/crontabs/ -p wa -k cron_changes

# Monitor network config
-w /etc/hosts -p wa -k hosts_changes
-w /etc/network/ -p wa -k network_changes

# Monitor startup scripts
-w /etc/init.d/ -p wa -k init_changes
-w /etc/rc.local -p wa -k init_changes

# Track all privileged command executions
-a always,exit -F arch=b64 -S execve -F euid=0 -k root_commands
-a always,exit -F arch=b32 -S execve -F euid=0 -k root_commands

# Monitor sudo usage
-w /usr/bin/sudo -p x -k sudo_usage

# Monitor failed login attempts
-a always,exit -F arch=b64 -S open -F exit=-EACCES -k access_denied
-a always,exit -F arch=b64 -S open -F exit=-EPERM -k access_denied

# Make audit config immutable (requires reboot to change rules)
-e 2
```

```bash
# Reload audit rules
sudo augenrules --load
sudo systemctl restart auditd
```

### Query Audit Logs
```bash
# See all events tagged 'sudo_usage'
sudo ausearch -k sudo_usage | aureport -i

# See authentication events
sudo ausearch -k auth_log

# Generate a summary report
sudo aureport --summary

# See all failed login attempts
sudo aureport --failed --auth

# See all root commands run
sudo ausearch -k root_commands | head -50
```

---

## 14. File Integrity Monitoring — AIDE

AIDE (Advanced Intrusion Detection Environment) creates a cryptographic fingerprint of every file on your system. If a hacker or malware modifies any file, AIDE will detect it.

```bash
# Install
sudo apt install aide aide-common -y

# Initialize the database AFTER you finish configuring everything
sudo aideinit

# Copy the new database to the active location
sudo cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db

# Run an integrity check
sudo aide --check

# Update the database (run after intentional system changes)
sudo aide --update
sudo cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db

# Automate nightly checks at 1 AM
echo "0 1 * * * root /usr/bin/aide --check 2>&1 | mail -s '[Kali] AIDE Integrity Report' root" | sudo tee -a /etc/crontab
```

> ⚠️ **Critical:** Initialize AIDE **after** you finish your full system setup. If you init too early, the baseline will be wrong and you'll get false positives.

---

## 15. Security Audit — Lynis

Lynis is a comprehensive security auditing tool. It scans your system and gives you a hardening score with specific, actionable recommendations.

```bash
# Install
sudo apt install lynis -y

# Update Lynis first
sudo lynis update info

# Run a full system audit
sudo lynis audit system

# Run audit and save report
sudo lynis audit system --report-file /var/log/lynis-report.dat

# View the report
sudo lynis show report
```

Lynis will give you a **Hardening Index score** (0–100) and a detailed list of what to fix. Aim for **70+** as a baseline.

---

## 16. Network Monitoring & Anti-Bot

### 🔍 Monitor Open Ports & Active Connections
```bash
# See all listening ports and services
sudo ss -tulnp

# See all active connections (great for spotting bots calling home)
sudo ss -tunp

# Older style (also works)
sudo netstat -tulnp

# Watch connections in real time
watch -n 2 'sudo ss -tunp'
```

### 🌍 Block Entire Country IP Ranges (GeoIP Blocking)
Bots often come from specific countries. Block them using `ipset`:
```bash
sudo apt install ipset xtables-addons-common libtext-csv-xs-perl -y

# Download and use country block lists (example: block known bad ranges)
# Visit: https://www.ipdeny.com/ipblocks/ for country-specific ranges
```

### 🕵️ Watch Network Traffic — TCPDump
```bash
# Capture all traffic on eth0
sudo tcpdump -i eth0 -n

# Capture only incoming connections
sudo tcpdump -i eth0 'tcp[tcpflags] & (tcp-syn) != 0' -n

# Save capture to file for analysis
sudo tcpdump -i eth0 -w /tmp/capture.pcap

# Watch for port scan attempts
sudo tcpdump -i eth0 -n 'tcp[tcpflags] == tcp-syn' | awk '{print $3}' | sort | uniq -c | sort -rn | head
```

### 🛑 Manually Block Suspicious IPs
```bash
# Block an IP using UFW
sudo ufw deny from 1.2.3.4 comment 'Suspicious bot'

# Block with iptables directly
sudo iptables -I INPUT -s 1.2.3.4 -j DROP

# Block an entire subnet
sudo iptables -I INPUT -s 1.2.3.0/24 -j DROP

# Make iptables rules persistent
sudo apt install iptables-persistent -y
sudo netfilter-persistent save
```

### 📊 Netstat Port Monitoring Script
Save this as `/usr/local/bin/check-ports.sh`:
```bash
#!/bin/bash
echo "=========================================="
echo "       OPEN PORTS & CONNECTIONS"
echo "=========================================="
echo ""
echo "--- Listening Ports ---"
sudo ss -tulnp
echo ""
echo "--- Active Connections ---"
sudo ss -tunp | grep ESTAB
echo ""
echo "--- Top Connection Sources ---"
sudo ss -tunp | awk '{print $6}' | grep -v Address | cut -d: -f1 | sort | uniq -c | sort -rn | head -20
```
```bash
sudo chmod +x /usr/local/bin/check-ports.sh
check-ports.sh
```

---

## 17. Disable Unnecessary Services

Every running service is a potential entry point for attackers. Minimize your attack surface ruthlessly.

### Check What's Running
```bash
# List all active services
sudo systemctl list-units --type=service --state=running

# List all enabled services (start at boot)
sudo systemctl list-unit-files --state=enabled
```

### Disable Services You Don't Need
```bash
# Bluetooth (unless needed)
sudo systemctl disable bluetooth --now

# CUPS Printing service
sudo systemctl disable cups --now
sudo systemctl disable cups-browsed --now

# Avahi (network discovery / mDNS — risky on public networks)
sudo systemctl disable avahi-daemon --now

# NFS (if not using network file sharing)
sudo systemctl disable nfs-server --now
sudo systemctl disable rpcbind --now

# Telnet, RSH, NIS (old insecure services — NEVER run these)
sudo apt --purge remove telnetd rsh-server nis yp-tools xinetd -y

# Check and disable any unknown services
sudo systemctl list-units --type=service --state=running | grep -v "kali-system\|NetworkManager\|ssh\|cron\|auditd\|ufw\|fail2ban\|apparmor"
```

---

## 18. Log Monitoring & Management

### 📜 Important Log Files to Know
| Log File | What It Records |
|---|---|
| `/var/log/auth.log` | All login attempts, SSH, sudo usage |
| `/var/log/syslog` | General system messages |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/ufw.log` | Firewall block/allow events |
| `/var/log/fail2ban.log` | Banned IPs by fail2ban |
| `/var/log/clamav/` | Antivirus scan results |
| `/var/log/audit/audit.log` | Audit daemon events |
| `/var/log/dpkg.log` | Package installs/removals |

### 👁️ Real-Time Log Monitoring Commands
```bash
# Watch all auth events (logins, SSH, sudo)
sudo tail -f /var/log/auth.log

# Watch firewall events
sudo tail -f /var/log/ufw.log

# Watch system logs
sudo journalctl -f

# Watch for failed logins only
sudo journalctl -f | grep -i "failed\|invalid\|error"

# Count failed SSH login attempts by IP
sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn | head -20

# See all successful logins
sudo grep "Accepted" /var/log/auth.log
```

### 🔒 Protect Log Files from Tampering
```bash
# Make critical log files append-only (even root can't delete them)
sudo chattr +a /var/log/auth.log
sudo chattr +a /var/log/syslog
sudo chattr +a /var/log/ufw.log

# Verify the immutable flag is set
sudo lsattr /var/log/auth.log
```

### ⚙️ Install Logwatch (Automated Log Reports)
```bash
sudo apt install logwatch -y

# Run a report now
sudo logwatch --output stdout --format text --range today

# Configure daily email reports
sudo nano /etc/cron.daily/00logwatch
```

---

## 19. Automatic Security Updates

### Enable Unattended Security Upgrades
```bash
sudo apt install unattended-upgrades apt-listchanges -y

# Configure
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

Make sure this is uncommented:
```
Unattended-Upgrade::Origins-Pattern {
    "origin=Debian,codename=${distro_codename},label=Debian-Security";
    "origin=Kali,codename=${distro_codename},label=Kali";
};

// Auto-remove unused packages
Unattended-Upgrade::Remove-Unused-Dependencies "true";

// Reboot if needed (at 3 AM)
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "03:00";
```

```bash
# Enable automatic updates
sudo dpkg-reconfigure --priority=low unattended-upgrades

# Test the configuration (dry run)
sudo unattended-upgrade --dry-run --debug
```

---

## 20. Encrypt Sensitive Data

### 🔐 Encrypt Home Directory
```bash
sudo apt install ecryptfs-utils -y

# Encrypt home directory (do this from another user or recovery)
sudo ecryptfs-migrate-home -u kali
```

### 🔐 Create an Encrypted Volume for Sensitive Files
```bash
# Install cryptsetup
sudo apt install cryptsetup -y

# Create an encrypted container file (2GB example)
dd if=/dev/urandom of=/home/kali/secure.img bs=1M count=2048
sudo cryptsetup luksFormat /home/kali/secure.img
sudo cryptsetup luksOpen /home/kali/secure.img secure_vault
sudo mkfs.ext4 /dev/mapper/secure_vault

# Mount it
sudo mkdir /mnt/vault
sudo mount /dev/mapper/secure_vault /mnt/vault
sudo chown kali:kali /mnt/vault

# When done, unmount and close
sudo umount /mnt/vault
sudo cryptsetup luksClose secure_vault
```

### 🔐 Use GPG to Encrypt Files
```bash
# Generate GPG key
gpg --full-generate-key

# Encrypt a file
gpg -e -r "Your Name" sensitive-file.txt

# Decrypt a file
gpg -d sensitive-file.txt.gpg > sensitive-file.txt
```

---

## 21. VM Snapshots & Backups

### 📸 Snapshot Strategy (QEMU/Virt-Manager)
Think of snapshots as save points. Create them **before** major changes.

| Snapshot Name | When to Take |
|---|---|
| `01-clean-install` | Right after first boot before any changes |
| `02-fully-updated` | After first update cycle |
| `03-security-hardened` | After completing this entire guide |
| `04-tools-installed` | After installing your pentesting tools |
| `pre-[operation]` | Before any major operation/test |

```bash
# Using virsh CLI to manage snapshots
# List snapshots
virsh snapshot-list kali-vm

# Create a snapshot
virsh snapshot-create-as kali-vm "03-security-hardened" "After hardening guide"

# Revert to a snapshot
virsh snapshot-revert kali-vm "03-security-hardened"

# Delete a snapshot
virsh snapshot-delete kali-vm "old-snapshot"
```

### 💾 Backup the qcow2 Image
```bash
# On your HOST machine, back up the disk image
sudo cp kali-linux-2025.1a-qemu-amd64.qcow2 kali-backup-$(date +%Y%m%d).qcow2

# Compress it
gzip kali-backup-$(date +%Y%m%d).qcow2
```

---

## 22. Install Essential Kali Tools

### 🧰 Toolset Options (Choose Based on Your Use)
```bash
# Minimal — Top 10 tools only (~1GB)
sudo apt install kali-tools-top10 -y

# Default — Standard pentesting suite (~3GB)
sudo apt install kali-linux-default -y

# Large — Most tools (~8GB)
sudo apt install kali-linux-large -y

# Everything — Full arsenal (~20GB)
sudo apt install kali-linux-everything -y
```

### 🔍 Specific Tool Categories
```bash
# Reconnaissance & OSINT
sudo apt install -y nmap masscan amass subfinder recon-ng theharvester

# Web Application Testing
sudo apt install -y burpsuite gobuster ffuf nikto sqlmap wfuzz dirb feroxbuster

# Password & Credential Attacks
sudo apt install -y john hashcat hydra medusa wordlists

# Wireless Testing
sudo apt install -y aircrack-ng wifite bettercap

# Exploitation Framework
sudo apt install -y metasploit-framework exploitdb

# Forensics & Analysis
sudo apt install -y autopsy volatility3 binwalk foremost

# Network Analysis
sudo apt install -y wireshark tcpdump netcat-openbsd socat

# Wordlists (SecLists — Every hacker needs this)
sudo apt install -y seclists
```

---

## 23. Quality of Life Setup

### 🖥️ Better Terminal & Productivity
```bash
# Terminal multiplexer (work in multiple panes/windows)
sudo apt install -y tmux terminator

# Better file tools
sudo apt install -y htop btop tree bat fd-find ripgrep fzf

# Screenshot tool
sudo apt install -y flameshot

# Password manager
sudo apt install -y keepassxc

# Text editors
sudo apt install -y vim nano gedit

# Network tools
sudo apt install -y curl wget jq net-tools dnsutils whois
```

### ⚡ Useful Bash Aliases
Add to `~/.bashrc`:
```bash
# === SYSTEM ===
alias update='sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y'
alias cls='clear'
alias root='sudo -i'
alias reload='source ~/.bashrc'

# === FILES ===
alias ls='ls --color=auto'
alias lss='ls -lah --color=auto'
alias lt='ls -lth --color=auto'
alias la='ls -A'

# === NAVIGATION ===
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

# === SECURITY ===
alias ports='sudo ss -tulnp'
alias myip='curl -s ifconfig.me && echo'
alias localip='hostname -I | awk "{print \$1}"'
alias connections='sudo ss -tunp | grep ESTAB'
alias scanme='sudo nmap -sV -O localhost'
alias fw='sudo ufw status verbose'
alias f2b='sudo fail2ban-client status'
alias logs='sudo tail -f /var/log/auth.log'
alias checkrootkit='sudo rkhunter --check --skip-keypress'

# === PENTESTING ===
alias msfstart='sudo msfdb init && sudo msfconsole'
alias burp='burpsuite &'

# === GIT ===
alias gs='git status'
alias gp='git push'
alias gl='git pull'
alias gc='git commit -m'
```

Apply:
```bash
source ~/.bashrc
```

### 🎨 Customize Bash Prompt (Show Git Branch & Status)
Add to `~/.bashrc`:
```bash
# Color prompt with username, host, path, git branch
parse_git_branch() {
    git branch 2>/dev/null | grep '*' | sed 's/* //'
}

export PS1='\[\033[01;31m\][\u@\h]\[\033[00m\] \[\033[01;34m\]\w\[\033[00m\]\[\033[01;33m\]$(parse_git_branch)\[\033[00m\] \$ '
```

### 🔑 SSH Key Management
```bash
# Generate key for different services
ssh-keygen -t ed25519 -C "github-key" -f ~/.ssh/github_ed25519
ssh-keygen -t ed25519 -C "server-key" -f ~/.ssh/server_ed25519

# SSH config file for easy connections
nano ~/.ssh/config
```

Add:
```ini
Host myserver
    HostName 192.168.1.100
    User kali
    Port 2222
    IdentityFile ~/.ssh/server_ed25519
    ServerAliveInterval 60
```

---

## 24. Security Maintenance Checklist

Run these regularly to keep your Kali VM secure:

### 📅 Daily
```bash
# Check failed login attempts
sudo grep "Failed password" /var/log/auth.log | tail -20

# Check what fail2ban has banned
sudo fail2ban-client status sshd

# Check open ports
sudo ss -tulnp

# Check for suspicious processes
ps auxf | grep -v '\[' | sort -k3 -rn | head -20
```

### 📅 Weekly
```bash
# Full system update
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y

# Rootkit scan
sudo rkhunter --update && sudo rkhunter --check --skip-keypress
sudo chkrootkit

# Antivirus scan
sudo freshclam && sudo clamscan -r /home/kali -i

# File integrity check
sudo aide --check

# Review active users
last | head -20
lastb | head -20  # Failed login attempts
```

### 📅 Monthly
```bash
# Full Lynis security audit
sudo lynis audit system

# Review all sudo usage
sudo ausearch -k sudo_usage | head -50

# Check for SUID/SGID files (potential privilege escalation)
sudo find / -perm /4000 -o -perm /2000 2>/dev/null | grep -v proc

# Check for world-writable files
sudo find / -perm -0002 -type f 2>/dev/null | grep -v proc

# Review cron jobs for all users
sudo cat /etc/crontab
sudo ls /etc/cron.d/
sudo ls /var/spool/cron/crontabs/

# Rotate SSH keys if needed
# Check password expiry
sudo chage -l kali
```

---

## 25. Quick Reference Cheatsheet

### 🔧 Install ALL Security Tools (One Command)
```bash
sudo apt update && sudo apt install -y \
  ufw fail2ban \
  apparmor apparmor-utils apparmor-profiles \
  rkhunter chkrootkit \
  clamav clamav-daemon \
  auditd audispd-plugins \
  aide aide-common \
  lynis \
  libpam-google-authenticator \
  libpam-pwquality \
  suricata \
  logwatch \
  unattended-upgrades \
  iptables-persistent \
  ecryptfs-utils cryptsetup \
  net-tools tcpdump wireshark \
  htop btop tree curl wget
```

---

### 📋 Security Layers Summary

| Layer | Tool | Purpose |
|---|---|---|
| 🔥 **Firewall** | UFW | Block all unwanted traffic |
| 🚫 **Anti-Brute Force** | Fail2Ban | Auto-ban attacking IPs |
| 🔑 **Authentication** | SSH Keys + 2FA | Prevent unauthorized access |
| 🛡️ **Access Control** | AppArmor | Confine programs |
| 🔍 **Rootkit Detection** | RKHunter + Chkrootkit | Find hidden malware |
| 🦠 **Antivirus** | ClamAV | Scan files for malware |
| 🕵️ **Network IDS** | Suricata | Detect network attacks |
| 📋 **Audit Logging** | Auditd | Track all system activity |
| 📁 **File Integrity** | AIDE | Detect file tampering |
| 🔒 **Kernel Hardening** | Sysctl | Anti-spoofing, anti-flood |
| 🔐 **Encryption** | LUKS / GPG | Protect sensitive data |
| 📊 **Security Audit** | Lynis | Score & audit your system |
| 📜 **Log Monitoring** | Logwatch + journalctl | Review system events |
| 🔄 **Auto Updates** | Unattended-Upgrades | Patch vulnerabilities auto |

---

### 🚨 Emergency Response Commands

If you think you've been compromised:
```bash
# Immediately block all incoming traffic
sudo ufw default deny incoming
sudo ufw reload

# Check for unusual processes
ps auxf | sort -k3 -rn | head -20

# Check for unusual network connections
sudo ss -tunp | grep ESTAB

# Check recent logins
last -n 20
lastb -n 20

# Check recently modified files
sudo find / -mtime -1 -type f 2>/dev/null | grep -v proc | grep -v sys

# Check crontabs for backdoors
sudo crontab -l
sudo cat /etc/crontab
sudo ls /etc/cron.d/

# Check for new user accounts
sudo cat /etc/passwd | tail -5

# Kill a suspicious process
sudo kill -9 <PID>

# Run rootkit scanner immediately
sudo rkhunter --check --skip-keypress
```

---

> 🧠 **Final Thought:** Security is not a product — it's a **process**. No single tool makes you secure. It's the combination of layered defenses (defense-in-depth), regular monitoring, and staying up to date that keeps you protected. Review this guide monthly, keep your tools updated, and always know what's running on your system.

---

*Guide maintained for Kali Linux 2025.1a on QEMU/KVM | Last updated: July 2026*
