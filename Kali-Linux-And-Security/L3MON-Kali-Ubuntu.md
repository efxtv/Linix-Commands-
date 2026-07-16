# Complete L3MON Setup Guide on Kali Linux

A comprehensive guide covering Kali Linux installation across multiple virtualization platforms, L3MON setup (native & Docker), and essential security hardening.

---

## Table of Contents

1. [What is L3MON?](#what-is-l3mon)
2. [Prerequisites](#prerequisites)
3. [Part 1: Kali Linux VM Setup](#part-1-kali-linux-vm-setup)
   - [VirtualBox](#a-virtualbox)
   - [VMware Workstation](#b-vmware-workstation)
   - [QEMU/KVM](#c-qemukvm)
4. [Part 2: Pre-Setup Security & System Hardening](#part-2-pre-setup-security--system-hardening)
5. [Part 3: Installing L3MON Dependencies](#part-3-installing-l3mon-dependencies)
6. [Part 4: Installing L3MON](#part-4-installing-l3mon)
7. [Part 5: Docker Installation for L3MON](#part-5-docker-installation-for-l3mon)
8. [Part 6: Security Hardening After Setup](#part-6-security-hardening-after-setup)
9. [Part 7: Networking Fundamentals & Remote Access](#part-7-networking-fundamentals--remote-access)
   - [Understanding Network Modes](#understanding-network-modes)
   - [VPN Solutions](#vpn-solutions)
   - [Reverse Tunneling & Port Forwarding](#reverse-tunneling--port-forwarding)
   - [Cloud VPS Setup (AWS)](#cloud-vps-setup-aws)
10. [Troubleshooting](#troubleshooting)
11. [Uninstall/Cleanup](#uninstallcleanup)

---

## What is L3MON?

**L3MON** is a web-based Android Remote Administration Tool (RAT) built with Node.js. It's designed for:
- **Ethical hacking & penetration testing**
- **Security research**
- **Remote device management** (for authorized devices only)

### Key Features:
- GPS Logging & Location Tracking
- Microphone Recording
- SMS Management (view, send, intercept)
- Call Log Access
- Contact Management
- Live Clipboard & Notification Logging
- WiFi Network Information
- File Explorer & Downloader
- Built-in APK Builder

### ⚠️ Important Disclaimer
> **This tool must ONLY be used on devices you own or have explicit written authorization to test. Unauthorized access to devices is illegal and unethical.**

---

## Prerequisites

| Requirement | Specification |
|-------------|---------------|
| **RAM** | Minimum 2GB (4GB recommended) |
| **Storage** | 20GB minimum |
| **Java** | **ONLY Java 1.8.0 / JRE 8** (other versions will break APK building) |
| **Node.js** | 14.x - 18.x LTS |
| **OS** | Kali Linux 2024.x or later |

---

## Part 1: Kali Linux VM Setup

### A. VirtualBox

#### Step 1: Download Required Files

1. Download Kali Linux ISO from [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
   - Choose **Kali Linux Installer** (recommended) or **Kali Linux VM** images

2. Download VirtualBox from [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

3. Download VirtualBox Extension Pack (for USB and other features)

#### Step 2: Install VirtualBox

**On Kali/Debian:**
```bash
sudo apt update
sudo apt install -y virtualbox virtualbox-ext-pack
```

**On Ubuntu:**
```bash
sudo apt update
sudo apt install -y virtualbox virtualbox-ext-pack
```

**On Fedora/RHEL:**
```bash
sudo dnf install -y VirtualBox
```

#### Step 3: Create New Virtual Machine

1. Open VirtualBox → Click **New**

2. Configure the VM:
   ```
   Name: Kali Linux
   Type: Linux
   Version: Debian (64-bit)
   Memory: 4096 MB (minimum 2048 MB)
   Hard disk: Create a virtual disk now
   ```

3. Click **Next** and configure storage:
   ```
   Hard disk file type: VDI (VirtualBox Disk Image)
   Storage on physical disk: Dynamically allocated
   File location: Default
   Size: 50 GB minimum (100 GB recommended)
   ```

4. Click **Create**

#### Step 4: Configure VM Settings

Right-click on the VM → **Settings**

**System → Processor:**
```
Processors: 2-4 (depending on your CPU)
Enable PAX/NX: Yes
Enable EFI: Optional (leave default)
```

**Display → Screen:**
```
Video Memory: 128 MB
Graphics Controller: VBoxSVGA
Enable 3D Acceleration: Yes (if supported)
```

**Network → Adapter 1:**
```
Attached to: NAT (for internet access)
Or use Bridged Adapter for LAN access to host
```

**Shared Folders (Optional):**
```
Add a shared folder for file transfer between host and VM
Enable Auto-mount: Yes
```

#### Step 5: Mount ISO and Install

1. Go to **Storage** → **Controller: IDE** → Click the CD icon
2. Select **Choose a disk file** → Browse to Kali ISO
3. Click **OK**
4. Start the VM and follow the graphical installer

#### Step 6: VirtualBox Guest Additions

After installation, install Guest Additions for better performance:

```bash
sudo apt update
sudo apt install -y virtualbox-guest-x11
# OR for full features:
sudo apt install -y virtualbox-guest-utils virtualbox-guest-x11 virtualbox-guest-dkms
reboot
```

---

### B. VMware Workstation

#### Step 1: Download Required Files

1. Download Kali Linux ISO from [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
2. Download VMware Workstation Player (free) or Pro from [https://www.vmware.com/products/workstation-pro.html](https://www.vmware.com/products/workstation-pro.html)

#### Step 2: Install VMware Workstation

**On Linux:**
```bash
# Make installer executable
chmod +x VMware-Workstation-Full-*.x86_64.bundle
sudo ./VMware-Workstation-Full-*.x86_64.bundle
```

**On Windows:** Run the installer as Administrator

#### Step 3: Create New Virtual Machine

1. Open VMware → **Create a New Virtual Machine**

2. Select **Typical (recommended)** → Click **Next**

3. Select **Installer disc image file (iso)** → Browse to Kali ISO → Click **Next**

4. Configure guest operating system:
   ```
   Guest operating system: Linux
   Version: Debian 11.x 64-bit (or latest available)
   ```

5. Name the VM:
   ```
   Virtual machine name: Kali Linux
   Location: Choose a location with sufficient space
   ```

6. Specify disk capacity:
   ```
   Maximum disk size: 100 GB
   Split virtual disk into multiple files: Yes
   ```

7. Click **Customize Hardware**:
   ```
   Memory: 4096 MB (minimum 2048 MB)
   Processors: 2-4 cores
   Network Adapter: NAT (or Bridged for LAN)
   Display: Auto-detect settings
   ```

8. Click **Close** → **Finish**

#### Step 4: Install Kali Linux

1. Power on the VM
2. Select **Graphical Install**
3. Follow the on-screen prompts
4. For partition: Choose **Guided - Use entire disk**
5. Complete installation and reboot

#### Step 5: VMware Tools Installation

```bash
# For VMware Workstation on Linux host
sudo apt update
sudo apt install -y open-vm-tools-desktop
reboot

# Alternative for full features
sudo apt install -y vmware-tools-linux
```

---

### C. QEMU/KVM

#### Step 1: Check Hardware Requirements

```bash
# Check if CPU supports virtualization
egrep -c '(vmx|svm)' /proc/cpuinfo
# Output should be > 0
```

#### Step 2: Install QEMU/KVM

**On Kali/Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

**Enable libvirtd:**
```bash
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```

#### Step 3: Create VM Using Virt-Manager (GUI)

1. Open Virt-Manager:
```bash
sudo virt-manager
```

2. Click **File** → **New Virtual Machine**

3. Select **Local install media (ISO image or CDROM)** → Click **Forward**

4. Browse to Kali Linux ISO → Check **Automatically detect operating system** → Click **Forward**

5. Configure resources:
   ```
   Memory (RAM): 4096 MB
   CPUs: 2-4
   ```
   → Click **Forward**

6. Create storage:
   ```
   Select or create custom storage
   Disk size: 100 GB
   ```
   → Click **Forward**

7. Name your VM and configure networking:
   ```
   Name: Kali-Linux
   Network selection: NAT or Bridge (depending on needs)
   ```
   → Click **Finish**

8. Complete the Kali installation through the VM window

#### Step 4: Create VM Using CLI (No GUI)

```bash
# Download Kali ISO (if not already)
wget -O kali-linux.iso https://cdimage.kali.org/kali-2024.1/kali-linux-2024.1-installer-amd64.iso

# Create qcow2 disk image
qemu-img create -f qcow2 kali-linux.qcow2 100G

# Install Kali Linux
sudo qemu-system-x86_64 \
  -m 4096 \
  -cdrom kali-linux.iso \
  -drive file=kali-linux.qcow2,format=qcow2 \
  -boot menu=on \
  -display gtk \
  -net nic \
  -net user,hostfwd=tcp::2222-:22
```

#### Step 5: Post-Installation Optimizations

```bash
# Install virtio drivers for better performance
sudo apt update
sudo apt install -y qemu-guest-agent
sudo systemctl enable qemu-guest-agent
reboot
```

---

## Part 2: Pre-Setup Security & System Hardening

Before installing any RAT tools, secure your Kali Linux installation.

### A. Initial System Updates

```bash
# Update all packages
sudo apt update && sudo apt upgrade -y

# Full distribution upgrade
sudo apt full-upgrade -y

# Clean up
sudo apt autoremove -y
sudo apt autoclean
```

### B. Configure UFW (Uncomplicated Firewall)

#### Install UFW
```bash
sudo apt update
sudo apt install -y ufw
```

#### Default Deny Policy
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

#### Enable UFW
```bash
sudo ufw enable
```

#### Verify Status
```bash
sudo ufw status verbose
```

### C. Disable Unnecessary Services

```bash
# Check running services
systemctl list-units --type=service --state=running

# Stop and disable unnecessary services
sudo systemctl stop cups                  # Print service (usually not needed)
sudo systemctl disable cups

sudo systemctl stop bluetooth              # Bluetooth (if not needed)
sudo systemctl disable bluetooth
sudo systemctl mask bluetooth              # Mask to prevent starting

sudo systemctl stop apache2                 # Web server (if pre-installed)
sudo systemctl disable apache2

sudo systemctl stop nginx                   # Nginx (if pre-installed)
sudo systemctl disable nginx
```

### D. Disable Risky Network Services

```bash
# SSH configuration (We'll configure secure SSH later)
sudo systemctl stop ssh
sudo systemctl disable ssh

# Stop any VNC servers
sudo systemctl stop x11vnc 2>/dev/null
sudo systemctl disable x11vnc 2>/dev/null
```

### E. Configure Secure SSH (If Needed)

If you need SSH access, secure it properly:

```bash
# Install SSH server
sudo apt install -y openssh-server

# Backup original config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# Configure secure SSH
sudo nano /etc/ssh/sshd_config
```

Make these changes:
```
Port 2222                               # Change from default 22
Protocol 2                              # Use SSH protocol 2 only
PermitRootLogin no                      # Disable root login
MaxAuthTries 3                          # Limit auth attempts
PubkeyAuthentication yes                # Use key-based auth
PasswordAuthentication no               # Disable password auth
X11Forwarding no                        # Disable X11 forwarding
PermitEmptyPasswords no                 # No empty passwords
ClientAliveInterval 300                 # Timeout
ClientAliveCountMax 2
```

```bash
# Restart SSH
sudo systemctl restart sshd

# Allow only in UFW (later we'll add this)
# For now, keep SSH disabled until properly configured
sudo systemctl stop ssh
sudo systemctl disable ssh
```

### F. Network Security Settings

```bash
# Disable ICMP redirects
echo "net.ipv4.conf.all.accept_redirects = 0" | sudo tee -a /etc/sysctl.conf
echo "net.ipv6.conf.all.accept_redirects = 0" | sudo tee -a /etc/sysctl.conf

# Disable source packet routing
echo "net.ipv4.conf.all.accept_source_route = 0" | sudo tee -a /etc/sysctl.conf
echo "net.ipv6.conf.all.accept_source_route = 0" | sudo tee -a /etc/sysctl.conf

# Apply changes
sudo sysctl -p
```

### G. Disable Unused Filesystems

```bash
echo "install cramfs /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
echo "install freevxfs /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
echo "install jffs2 /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
echo "install hfs /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
echo "install hfsplus /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
echo "install squashfs /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
echo "install udf /bin/true" | sudo tee -a /etc/modprobe.d/CIS.conf
```

### H. Enable Automatic Security Updates

```bash
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

---

## Part 3: Installing L3MON Dependencies

### A. Java 8 Installation (CRITICAL)

L3MON requires **EXACTLY Java 1.8.0**. Other versions will break APK building.

> ⚠️ **IMPORTANT:** Kali Linux does not have Java 8 in its default repositories. You must manually download and install it.

#### Step 1: Download Java 8 JDK

**Option A: Download from Oracle (Official)**
1. Go to: [https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)
2. Download: **jdk-8uXXX-linux-x64.tar.gz** (you need an Oracle account)
3. Transfer the file to your Kali machine

**Option B: Download from Eclipse Temurin (Free/Open Source) - RECOMMENDED**
```bash
cd /tmp
wget https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u432-b06/OpenJDK8U-jdk_x64_linux_hotspot_8u432b06.tar.gz
```

> 📌 **Note:** Visit [https://adoptium.net/temurin/releases/?version=8](https://adoptium.net/temurin/releases/?version=8) for the latest Java 8 releases.

#### Step 2: Extract the Archive

```bash
# Navigate to where you downloaded the file
cd /tmp

# Extract to /opt directory
sudo tar -xzf OpenJDK8U-jdk_x64_linux_hotspot_8u432b06.tar.gz -C /opt/

# Verify extraction
ls -la /opt/ | grep jdk
```

#### Step 3: Set Up Java Alternatives

```bash
# Register Java with update-alternatives
sudo update-alternatives --install "/usr/bin/java" "java" "/opt/jdk8u432-b06/bin/java" 1

sudo update-alternatives --install "/usr/bin/javac" "javac" "/opt/jdk8u432-b06/bin/javac" 1

sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/opt/jdk8u432-b06/bin/javaws" 1
```

#### Step 4: Configure Default Java Version

```bash
# Select Java 8 as the default
sudo update-alternatives --config java

# You will see a prompt like this:
# There are 3 choices for the alternative java (providing /usr/bin/java).
# 
#   Selection    Path                                      Priority   Status
# ------------------------------------------------------------
# * 0            /usr/lib/jvm/java-17-openjdk-amd64/bin/java   1711      auto mode
#   1            /opt/jdk8u432-b06/bin/java                   1         manual mode
#   2            /usr/lib/jvm/java-11-openjdk-amd64/bin/java  1111      manual mode
#   3            /usr/lib/jvm/java-17-openjdk-amd64/bin/java  1711      manual mode
#
# Press <enter> to keep the current choice*, or type selection number: 1

# Enter the number for your Java 8 installation (e.g., 1)

# Also configure javac
sudo update-alternatives --config javac
# Select Java 8
```

#### Step 5: Set Environment Variables

```bash
# Create environment file
sudo nano /etc/profile.d/java8.sh
```

Add the following content:
```bash
# Java 8 Environment Variables
export JAVA_HOME=/opt/jdk8u432-b06
export PATH=$JAVA_HOME/bin:$PATH
```

```bash
# Make it executable
sudo chmod +x /etc/profile.d/java8.sh

# Apply the environment variables
source /etc/profile.d/java8.sh
```

#### Step 6: Verify Installation

```bash
# Verify Java version
java -version
```

**Expected output:**
```
openjdk version "1.8.0_432"  (for Temurin)
java version "1.8.0_xxx"     (for Oracle)
OpenJDK Runtime Environment (build 1.8.0_432)
OpenJDK 64-Bit Server VM (build ...)
OpenJDK Runtime Environment (build jdk8u432-b06)
```

```bash
# Verify javac
javac -version

# Check JAVA_HOME
echo $JAVA_HOME
# Should output: /opt/jdk8u432-b06
```

#### Troubleshooting Java Installation

**If java -version shows wrong version:**
```bash
# Check which java is being used
which java
ls -la /usr/bin/java

# Force update-alternatives
sudo update-alternatives --set java /opt/jdk8u432-b06/bin/java
sudo update-alternatives --set javac /opt/jdk8u432-b06/bin/javac
```

**If JAVA_HOME is not set after reboot:**
```bash
# Add to ~/.bashrc for persistence
echo 'export JAVA_HOME=/opt/jdk8u432-b06' >> ~/.bashrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

**Verify Java works with apktool:**
```bash
# Test apktool installation
apktool --version
# Should work with Java 8
```

### B. Node.js Installation

#### Method 1: Nodesource Repository (Recommended)

```bash
# Install Node.js 18.x LTS
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo bash -
sudo apt install -y nodejs

# Verify
node --version   # Should show v18.x.x
npm --version    # Should show 9.x.x or 10.x.x
```

#### Method 2: APT Default

```bash
sudo apt update
sudo apt install -y nodejs npm

# Verify versions
node --version
npm --version
```

### C. Install Build Tools

```bash
sudo apt install -y build-essential python3 git curl wget nano jq openssl
```

### D. Install apktool (Required for APK Building)

```bash
# Install apktool for L3MON
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh)
```

### E. Verify All Dependencies

```bash
# Check all installed tools
echo "=== Dependency Check ==="
echo "Java:"
java -version 2>&1 | head -1

echo "Node.js:"
node --version

echo "npm:"
npm --version

echo "Git:"
git --version

echo "OpenSSL:"
openssl version

echo "apktool:"
apktool 2>&1 | head -1
```

---

## Part 4: Installing L3MON

### A. Clone L3MON Repository

```bash
# Create directory for L3MON
mkdir -p ~/tools
cd ~/tools

# Clone the repository
git clone https://github.com/efxtv/L3MON.git
cd L3MON
```

### B. Install PM2 Process Manager

```bash
# Install PM2 globally
sudo npm install pm2 -g

# Verify installation
pm2 --version
```

### C. Install Node Dependencies

```bash
# Install project dependencies
npm install

# Run security audit and fix
npm audit
npm audit fix
npm audit fix --force
```

**If you get node-gyp or sqlite errors:**
```bash
sudo apt install -y build-essential python3
npm install --build-from-source
```

### D. Start L3MON for First Time

```bash
# Start L3MON
pm2 start index.js --name l3mon

# Check status
pm2 status
```

### E. Configure Admin Login

**Generate MD5 Password:**
```bash
# Generate MD5 hash (must be lowercase)
echo -n "YourSecurePassword123" | openssl md5 | awk '{print $2}'
```

**Example output:**
```
# echo -n "MySecurePass" | openssl md5
# (stdin)= 5d41402abc4b2a76b9719d911017c592
```

**Edit maindb.json:**
```bash
# Stop L3MON
pm2 stop l3mon

# Edit configuration
nano maindb.json
```

Update the admin section:
```json
{
  "admin": {
    "username": "admin",
    "password": "5d41402abc4b2a76b9719d911017c592"
  }
}
```

**Save and exit:** Press `Ctrl+O`, then `Ctrl+X`

### F. Set Up PM2 Startup

```bash
# Restart L3MON
pm2 restart l3mon

# Save process list
pm2 save

# Generate startup script
pm2 startup

# Copy and run the command it outputs (example)
# sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u $USER --hp /home/USER
```

### G. Access L3MON Dashboard

```bash
# Check if running
pm2 status

# View logs
pm2 logs l3mon
```

Access the dashboard:
```
http://127.0.0.1:22533
http://localhost:22533
```

Login with your configured username and password.

---

## Part 5: Docker Installation for L3MON

### A. Install Docker

#### On Kali Linux (Debian-based)

```bash
# Update package index
sudo apt update

# Install dependencies
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list

# Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add current user to docker group
sudo usermod -aG docker $USER

# Log out and back in for group changes to take effect
# OR use:
newgrp docker
```

#### Alternative: Convenience Script

```bash
# Download and run Docker convenience script
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### B. Docker Commands

```bash
# Check Docker is installed
docker --version

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Verify Docker is working
sudo docker run hello-world
```

### C. L3MON Docker Setup (Official Docker Hub Image)

The easiest method - use the official L3MON image from Docker Hub.

#### Step 1: Install Docker

If Docker is not installed:

```bash
# Update package index
sudo apt update

# Install dependencies
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list

# Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add current user to docker group (no sudo needed after this)
sudo usermod -aG docker $USER

# Log out and back in, OR run:
newgrp docker

# Start Docker
sudo systemctl start docker
sudo systemctl enable docker

# Verify
docker --version
```

#### Step 2: Pull L3MON Image from Docker Hub

```bash
# Pull the official L3MON image
docker pull efxtv/l3mon

# Verify image is downloaded
docker images
```

**Expected output:**
```
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
efxtv/l3mon   latest    xxxxxxxxxxxx   ...           ...
```

#### Step 3: Configure Firewall (UFW) for L3MON Ports

Before running the container, configure your firewall to allow the required ports:

```bash
# Install UFW if not installed
sudo apt install -y ufw

# Reset UFW to defaults
sudo ufw reset

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow L3MON Dashboard (Web Interface) - Port 22533
sudo ufw allow 22533/tcp comment 'L3MON Dashboard'

# Allow L3MON Client Connection Port (Android APK clients) - Port 22222
sudo ufw allow 22222/tcp comment 'L3MON Client Port'

# Allow SSH on custom port (example: 2222) - Optional
sudo ufw allow 2222/tcp comment 'SSH'

# Enable UFW
sudo ufw enable

# Verify rules
sudo ufw status numbered
```

**Expected UFW output:**
```
Status: active

To                         Action      From
--                         ------      ----
22533/td>                  ALLOW       Anywhere
22222/tcp                  ALLOW       Anywhere
2222/tcp                   ALLOW       Anywhere
```

#### Step 4: Run L3MON Docker Container

**Method A: Basic Run (Simple)**

```bash
# Run L3MON container with port mapping
docker run -d \
  --name l3mon \
  -p 22533:22533 \
  -p 22222:22222 \
  efxtv/l3mon
```

**Method B: Run with Persistent Data (Recommended)**

```bash
# Create directories for persistent data on host
mkdir -p ~/l3mon-data

# Run container with volume mounts (data persists after container restart)
docker run -d \
  --name l3mon \
  -p 22533:22533 \
  -p 22222:22222 \
  -v ~/l3mon-data:/L3MON/data \
  efxtv/l3mon
```

**Method C: Interactive Run (Full Control)**

```bash
# Run container in interactive mode (for full control)
docker run -it \
  --name l3mon \
  -p 22533:22533 \
  -p 22222:22222 \
  -v ~/l3mon-data:/L3MON/data \
  efxtv/l3mon \
  /bin/bash
```

#### Step 5: Access Docker Container Shell

If you used Method A or B, access the container's bash to configure L3MON:

```bash
# Access running container's bash shell
docker exec -it l3mon /bin/bash

# You should now see:
# root@container_id:/L3MON#
```

#### Step 6: Start L3MON with PM2

Inside the Docker container:

```bash
# Navigate to L3MON directory
cd /L3MON

# Start L3MON with PM2
pm2 start index.js

# Verify it's running
pm2 status
```

**Expected PM2 output:**
```
┌────┬────────────────┬─────────────┬──────┬──────┬────────┬─────────┬──────────┐
│ id │ name           │ mode       │ ↺    │ status│ cpu    │ memory │ watching│
├────┼────────────────┼─────────────┼──────┼──────┼────────┼─────────┼──────────┤
│ 0  │ index          │ fork       │ 0    │ online│ 0%     │ xxx MB  │ disabled │
└────┴────────────────┴─────────────┴──────┴──────┴────────┴─────────┴──────────┘
```

#### Step 7: Configure Admin Login (maindb.json)

**First, stop the current process:**

```bash
# Stop L3MON
pm2 stop index
```

**Generate MD5 Password:**

```bash
# Generate MD5 hash of your password (must be lowercase)
echo -n "YourSecurePassword123" | openssl md5 | awk '{print $2}'
```

**Example:**
```
echo -n "MySecretPass" | openssl md5 | awk '{print $2}'
# Output: 5d41402abc4b2a76b9719d911017c592
```

**Edit maindb.json:**

```bash
# View current maindb.json
cat maindb.json
```

**Default maindb.json structure:**
```json
{
  "admin": {
    "username": "admin",
    "password": "5d41402abc4b2a76b9719d911017c592"
  }
}
```

```bash
# Edit the file
nano maindb.json
```

**Update with your credentials:**
```json
{
  "admin": {
    "username": "your_admin_username",
    "password": "YOUR_GENERATED_MD5_HASH"
  }
}
```

**For example:**
```json
{
  "admin": {
    "username": "admin",
    "password": "3c8e4f8e5c1c8f7e9b0a1d2c3e4f5a6b"
  }
}
```

**Save and exit:** Press `Ctrl+O`, then `Enter`, then `Ctrl+X`

#### Step 8: Restart L3MON

```bash
# Restart L3MON
pm2 restart index

# Or if deleted:
pm2 start index.js

# Save PM2 process list
pm2 save

# View logs to verify it's running
pm2 logs index
```

#### Step 9: Exit Container (Background Mode)

If running in detached mode (Method A or B):

```bash
# Exit the container shell
exit

# Container continues running in background
# Verify container is running
docker ps
```

#### Step 10: Access L3MON Dashboard

**From the same machine (localhost):**
```
http://127.0.0.1:22533
http://localhost:22533
```

**From external network (replace with your server IP):**
```
http://YOUR_SERVER_IP:22533
```

**Login with the credentials you configured in maindb.json:**
- **Username:** your_admin_username
- **Password:** YourSecurePassword123 (the original password, NOT the MD5 hash)

---

### D. Docker Management Commands

#### Container Lifecycle

```bash
# View running containers
docker ps

# View all containers (including stopped)
docker ps -a

# Start container
docker start l3mon

# Stop container
docker stop l3mon

# Restart container
docker restart l3mon

# Remove container (must stop first)
docker stop l3mon
docker rm l3mon

# View container logs (follow mode)
docker logs -f l3mon

# View only last 50 lines
docker logs --tail 50 l3mon
```

#### Access Container Shell

```bash
# Access bash of running container
docker exec -it l3mon /bin/bash

# Access as root user
docker exec -u 0 -it l3mon /bin/bash

# Run single command inside container
docker exec l3mon pm2 status
docker exec l3mon cat /L3MON/maindb.json
```

#### Managing L3MON Inside Container

```bash
# Access container
docker exec -it l3mon /bin/bash

# Then inside container:
cd /L3MON

# Start L3MON
pm2 start index.js

# Stop L3MON
pm2 stop index

# Restart L3MON
pm2 restart index

# View logs
pm2 logs index

# Monitor in real-time
pm2 monit

# Delete and recreate
pm2 delete index
pm2 start index.js

# Save PM2 state
pm2 save
```

#### Networking & Ports

```bash
# Check port mappings
docker port l3mon

# Test if ports are accessible locally
curl http://localhost:22533
nc -zv localhost 22533
nc -zv localhost 22222

# Check what process is listening on ports
sudo lsof -i :22533
sudo lsof -i :22222
```

#### Data Persistence

```bash
# Check mounted volumes
docker inspect l3mon | grep -A 10 "Mounts"

# Backup data from container
docker cp l3mon:/L3MON/maindb.json ~/l3mon-backup.json
docker cp l3mon:/L3MON/clientData ~/l3mon-clientData-backup

# Restore data to container
docker cp ~/l3mon-backup.json l3mon:/L3MON/maindb.json
```

#### Complete Docker L3MON Setup (One-File Script)

Create and run this script for quick setup:

```bash
nano ~/l3mon-docker-setup.sh
```

```bash
#!/bin/bash

echo "=========================================="
echo "  L3MON Docker Setup Script"
echo "=========================================="
echo ""

# Variables
CONTAINER_NAME="l3mon"
PORTS_DASHBOARD="22533"
PORTS_CLIENT="22222"
DATA_DIR="$HOME/l3mon-data"

# Step 1: Check if Docker is installed
echo "[1/7] Checking Docker installation..."
if ! command -v docker &> /dev/null; then
    echo "Docker not found. Installing..."
    sudo apt update && sudo apt install -y docker.io
fi
echo "✓ Docker is installed"

# Step 2: Configure Firewall
echo "[2/7] Configuring UFW firewall..."
sudo apt install -y ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow $PORTS_DASHBOARD/tcp comment 'L3MON Dashboard'
sudo ufw allow $PORTS_CLIENT/tcp comment 'L3MON Client Port'
sudo ufw enable -y
echo "✓ Firewall configured"

# Step 3: Pull L3MON image
echo "[3/7] Pulling L3MON image from Docker Hub..."
docker pull efxtv/l3mon
echo "✓ Image downloaded"

# Step 4: Stop and remove existing container
echo "[4/7] Cleaning up existing container..."
docker stop $CONTAINER_NAME 2>/dev/null
docker rm $CONTAINER_NAME 2>/dev/null
echo "✓ Container cleaned"

# Step 5: Create data directory
echo "[5/7] Creating data directory..."
mkdir -p $DATA_DIR
echo "✓ Directory created: $DATA_DIR"

# Step 6: Run container
echo "[6/7] Starting L3MON container..."
docker run -d \
  --name $CONTAINER_NAME \
  -p $PORTS_DASHBOARD:$PORTS_DASHBOARD \
  -p $PORTS_CLIENT:$PORTS_CLIENT \
  -v $DATA_DIR:/L3MON/data \
  efxtv/l3mon
echo "✓ Container started"

# Step 7: Configure L3MON
echo "[7/7] Configuring L3MON..."
sleep 3
docker exec -it $CONTAINER_NAME /bin/bash -c "
    cd /L3MON
    pm2 start index.js
    pm2 stop index
    echo 'Enter your admin username:'
    read username
    echo 'Enter your password:'
    read -s password
    md5hash=\$(echo -n \$password | openssl md5 | awk '{print \$2}')
    cat > maindb.json << EOF
{
  \"admin\": {
    \"username\": \"\$username\",
    \"password\": \"\$md5hash\"
  }
}
EOF
    pm2 restart index
    pm2 save
"

echo ""
echo "=========================================="
echo "  Setup Complete!"
echo "=========================================="
echo ""
echo "Dashboard URL: http://127.0.0.1:$PORTS_DASHBOARD"
echo "Container shell: docker exec -it $CONTAINER_NAME /bin/bash"
echo "View logs: docker logs -f $CONTAINER_NAME"
echo "PM2 status: docker exec $CONTAINER_NAME pm2 status"
echo ""
echo "⚠️  Remember to open ports $PORTS_DASHBOARD and $PORTS_CLIENT in your router/cloud firewall!"
echo ""
```

```bash
# Make executable and run
chmod +x ~/l3mon-docker-setup.sh
sudo ~/l3mon-docker-setup.sh
```

#### Remove Docker L3MON

```bash
# Stop and remove container
docker stop l3mon
docker rm l3mon

# Remove image
docker rmi efxtv/l3mon

# Remove data directory (careful!)
rm -rf ~/l3mon-data

# Remove Docker if needed
sudo systemctl stop docker
sudo apt purge docker.io
sudo rm -rf /var/lib/docker
```

---

## Part 6: Security Hardening After Setup

### A. UFW Firewall Configuration

#### Configure UFW for L3MON

```bash
# Reset UFW to defaults
sudo ufw reset

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow L3MON Dashboard (Web Interface)
sudo ufw allow 22533/tcp comment 'L3MON Dashboard'

# Allow L3MON Client Connection Port (for Android APK connections)
sudo ufw allow 22222/tcp comment 'L3MON Client Port'

# Allow SSH on custom port (example: 2222)
sudo ufw allow 2222/tcp comment 'SSH'

# Deny common risky ports
sudo ufw deny 22/tcp comment 'Block default SSH'
sudo ufw deny 23/tcp comment 'Block Telnet'
sudo ufw deny 21/tcp comment 'Block FTP'
sudo ufw deny 3389/tcp comment 'Block RDP'
sudo ufw deny 3306/tcp comment 'Block MySQL'
sudo ufw deny 5432/tcp comment 'Block PostgreSQL'
sudo ufw deny 27017/tcp comment 'Block MongoDB'
sudo ufw deny 6379/tcp comment 'Block Redis'
sudo ufw deny 11211/tcp comment 'Block Memcached'
```

#### Enable UFW

```bash
# Enable UFW
sudo ufw enable

# Verify rules
sudo ufw status numbered

# Show verbose status
sudo ufw status verbose
```

**Expected output:**
```
Status: active

To                         Action      From
--                         ------      ----
22533/tcp                  ALLOW       Anywhere
22222/tcp                  ALLOW       Anywhere
2222/tcp                   ALLOW       Anywhere
22/tcp                     DENY         Anywhere
23/tcp                     DENY         Anywhere
21/tcp                     DENY         Anywhere
3389/tcp                   DENY         Anywhere
3306/tcp                   DENY         Anywhere
5432/tcp                   DENY         Anywhere
27017/tcp                  DENY         Anywhere
6379/tcp                   DENY         Anywhere
11211/tcp                  DENY         Anywhere
```

### B. Configure Secure SSH (Custom Port)

```bash
# Generate SSH key pair
ssh-keygen -t ed25519 -C "kali-l3mon-$(date +%Y%m%d)"

# Copy public key to authorized_keys
mkdir -p ~/.ssh
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# Backup and configure sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
sudo nano /etc/ssh/sshd_config
```

Configure these settings:
```
Port 2222
Protocol 2
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key

PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
X11Forwarding no
AllowTcpForwarding no
AllowAgentForwarding no
PrintMotd no
TCPKeepAlive yes

# Only allow specific users
AllowUsers your_username
```

```bash
# Generate new host keys
sudo ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
sudo ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key -N ""

# Restart SSH
sudo systemctl restart sshd
sudo systemctl enable sshd
```

### C. Install and Configure Fail2Ban

```bash
# Install Fail2Ban
sudo apt update
sudo apt install -y fail2ban

# Create local configuration
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

# Configure jail settings
sudo nano /etc/fail/fail2ban/jail.local
```

Add or modify these settings:
```ini
[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 3

[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
bantime = 86400
maxretry = 3
```

```bash
# Start and enable Fail2Ban
sudo systemctl start fail2ban
sudo systemctl enable fail2ban

# Check status
sudo fail2ban-client status
```

### D. Configure IP Tables (Advanced)

For more granular control, use iptables:

```bash
# Save current rules
sudo iptables-save > ~/iptables.backup.rules

# Create new rules script
sudo nano /usr/local/bin/firewall.sh
```

```bash
#!/bin/bash
# Firewall script for L3MON Server

# Flush existing rules
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

# Default policies
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# Allow loopback
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

# Allow established connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow L3MON Dashboard
iptables -A INPUT -p tcp --dport 22533 -j ACCEPT

# Allow L3MON Client Port
iptables -A INPUT -p tcp --dport 22222 -j ACCEPT

# Allow SSH on custom port
iptables -A INPUT -p tcp --dport 2222 -j ACCEPT

# Log dropped packets (optional)
iptables -A INPUT -j LOG --log-prefix "IPT_DROP: "

# Save rules
iptables-save > /etc/iptables/rules.v4
```

```bash
# Make executable and run
sudo chmod +x /usr/local/bin/firewall.sh
sudo /usr/local/bin/firewall.sh
```

### E. Secure L3MON Configuration

#### Update maindb.json

```bash
cd ~/tools/L3MON
pm2 stop l3mon
nano maindb.json
```

Add additional security settings:
```json
{
  "settings": {
    "port": 22533,
    "autoDelete": false,
    "maxClients": 100
  },
  "admin": {
    "username": "secure_admin",
    "password": "YOUR_MD5_HASHED_PASSWORD"
  }
}
```

#### Enable SSL/HTTPS (Optional)

For production, consider using a reverse proxy with SSL:

```bash
# Install Nginx
sudo apt install -y nginx

# Create SSL certificate
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/nginx-selfsigned.key \
  -out /etc/ssl/certs/nginx-selfsigned.crt

# Configure reverse proxy
sudo nano /etc/nginx/sites-available/l3mon
```

```nginx
server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name your-domain.com;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
    
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    
    location / {
        proxy_pass http://127.0.0.1:22533;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```bash
# Enable the site
sudo ln -s /etc/nginx/sites-available/l3mon /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
sudo ufw allow 443/tcp comment 'HTTPS'
```

### F. System Logging and Monitoring

```bash
# Configure rsyslog for L3MON
sudo nano /etc/rsyslog.d/30-l3mon.conf
```

```
# Log L3MON activity
local0.*    /var/log/l3mon.log
```

```bash
# Create logrotate config
sudo nano /etc/logrotate.d/l3mon
```

```
/var/log/l3mon.log {
    weekly
    rotate 4
    compress
    delaycompress
    missingok
    notifempty
    create 644 root root
}
```

```bash
# Restart rsyslog
sudo systemctl restart rsyslog
```

### G. Daily Security Tasks Script

Create an automated security check script:

```bash
nano ~/security-check.sh
```

```bash
#!/bin/bash
# Daily Security Check Script

LOGFILE="$HOME/security-check.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')

echo "=== Security Check - $DATE ===" >> $LOGFILE

# Check failed SSH attempts
echo "--- Failed SSH Attempts ---" >> $LOGFILE
grep "Failed password" /var/log/auth.log | tail -10 >> $LOGFILE

# Check UFW status
echo "--- UFW Status ---" >> $LOGFILE
ufw status verbose >> $LOGFILE

# Check running services
echo "--- Running Services ---" >> $LOGFILE
systemctl list-units --type=service --state=running | grep -E "(ssh|docker|pm2)" >> $LOGFILE

# Check PM2 status
echo "--- PM2 Status ---" >> $LOGFILE
pm2 status >> $LOGFILE 2>&1

# Check open ports
echo "--- Open Ports ---" >> $LOGFILE
ss -tulpn | grep LISTEN >> $LOGFILE

# Check disk usage
echo "--- Disk Usage ---" >> $LOGFILE
df -h >> $LOGFILE

echo "=== Check Complete ===" >> $LOGFILE
echo "" >> $LOGFILE

# Alert if suspicious activity
FAILED_ATTEMPTS=$(grep "Failed password" /var/log/auth.log | grep "$(date '+%b %d')" | wc -l)
if [ $FAILED_ATTEMPTS -gt 20 ]; then
    echo "WARNING: High number of failed SSH attempts detected!" | tee -a $LOGFILE
fi
```

```bash
chmod +x ~/security-check.sh

# Add to crontab for daily execution
(crontab -l 2>/dev/null; echo "0 2 * * * ~/security-check.sh") | crontab -
```

---

## Part 7: Networking Fundamentals & Remote Access

Understanding networking is crucial for L3MON because the Android APK clients need to connect back to your server. This section covers essential networking concepts and solutions.

---

### Understanding Network Modes

#### NAT (Network Address Translation)

**What it is:** The VM shares the host's IP address. The host acts as a router, translating addresses.

**Pros:**
- Simple setup
- Automatic internet access
- Good security (VM is not directly accessible from network)

**Cons:**
- External devices cannot directly connect to the VM
- APK clients on the internet cannot reach your L3MON server

**Use case for L3MON:** ⚠️ **NOT RECOMMENDED** for L3MON server unless using tunneling solutions below.

```bash
# In VirtualBox: Settings → Network → Attached to: NAT
# In VMware: Network Adapter → NAT: Used to share host's IP
```

#### Bridged Networking

**What it is:** The VM gets its own IP on the local network, appearing as a separate device.

**Pros:**
- VM is directly accessible from the network
- APK clients on LAN can connect easily
- Better performance than NAT

**Cons:**
- VM is exposed to network (requires firewall)
- May conflict with DHCP servers

**Use case for L3MON:** ✅ **GOOD FOR LOCAL TESTING** on your network.

```bash
# VirtualBox: Settings → Network → Bridged Adapter → Select your NIC
# VMware: Network Adapter → Bridged (Replicate physical network state)
```

**Configuration:**
```bash
# Check your network interface
ip a
# Typical interfaces: eth0, enp0s3, wlan0, eth1 (for VM bridge)

# Set static IP (optional but recommended)
sudo nano /etc/network/interfaces
```

```bash
# Example static IP configuration
auto eth0
iface eth0 inet static
address 192.168.1.100
netmask 255.255.255.0
gateway 192.168.1.1
dns-nameservers 8.8.8.8 8.8.4.4
```

```bash
# Restart networking
sudo systemctl restart networking
# Or for NetworkManager:
sudo nmcli connection down "Wired connection 1" && sudo nmcli connection up "Wired connection 1"
```

#### Host-Only Networking

**What it is:** VM can only communicate with the host system, not the internet or other network devices.

**Pros:**
- Maximum isolation
- Good for testing
- Secure

**Cons:**
- No internet access in VM
- Cannot connect from external devices
- APK clients cannot reach server

**Use case for L3MON:** ⚠️ **NOT SUITABLE** - testing only.

#### Internal Network

**What it is:** VMs can communicate with each other but not with host or external network.

**Pros:**
- Isolated from everything
- VMs can communicate

**Cons:**
- No external access

---

### Port Forwarding Fundamentals

Port forwarding allows external traffic to reach your VM through the host.

#### VirtualBox Port Forwarding

```bash
# Using VBoxManage CLI
VBoxManage modifyvm "Kali Linux" --natpf1 "L3MON-Dashboard,tcp,,22533,,22533"
VBoxManage modifyvm "Kali Linux" --natpf1 "L3MON-Client,tcp,,22222,,22222"
```

**GUI Method:**
1. VM Settings → Network → Advanced → Port Forwarding
2. Add rules:
   ```
   Name: L3MON Dashboard
   Protocol: TCP
   Host Port: 22533
   Guest Port: 22533
   
   Name: L3MON Client
   Protocol: TCP
   Host Port: 22222
   Guest Port: 22222
   ```

#### QEMU/KVM Port Forwarding

```bash
# When starting QEMU with CLI
qemu-system-x86_64 \
  -m 4096 \
  -hda kali-linux.qcow2 \
  -netdev user,id=net0,hostfwd=tcp::22533-:22533,hostfwd=tcp::22222-:22222 \
  -device virtio-net-pci,netdev=net0
```

---

### VPN Solutions

A VPN creates a secure tunnel between your devices, allowing APK clients to connect from anywhere.

#### OpenVPN

**What it is:** Traditional open-source VPN solution. You run a VPN server and connect clients to it.

**Setup on Kali (Server):**

```bash
# Install OpenVPN
sudo apt update
sudo apt install -y openvpn easy-rsa

# Setup PKI
sudo mkdir -p /etc/openvpn/easy-rsa-keys
sudo cp -r /usr/share/easy-rsa/* /etc/openvpn/easy-rsa-keys/
cd /etc/openvpn/easy-rsa-keys

# Initialize PKI
sudo ./easyrsa init-pki
sudo ./easyrsa build-ca
sudo ./easyrsa gen-req server nopass
sudo ./easyrsa sign server server
sudo ./easyrsa gen-dh
sudo openvpn --genkey secret /etc/openvpn/ta.key

# Create server config
sudo nano /etc/openvpn/server.conf
```

```bash
# OpenVPN server configuration
port 1194
proto udp
dev tun
ca /etc/openvpn/easy-rsa-keys/pki/ca.crt
cert /etc/openvpn/easy-rsa-keys/pki/issued/server.crt
key /etc/openvpn/easy-rsa-keys/pki/private/server.key
dh /etc/openvpn/easy-rsa-keys/pki/dh.pem
tls-auth /etc/openvpn/ta.key 0
server 10.8.0.0 255.255.255.0
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
keepalive 10 120
cipher AES-256-CBC
auth SHA256
persist-key
persist-tun
status openvpn-status.log
verb 3
```

```bash
# Enable IP forwarding
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# Configure firewall
sudo iptables -A FORWARD -i tun0 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o tun0 -j ACCEPT
sudo iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE

# Start OpenVPN
sudo systemctl start openvpn@server
sudo systemctl enable openvpn@server
```

**Client Configuration:**

```bash
# Create client config file
nano client.ovpn
```

```bash
client
dev tun
proto udp
remote YOUR_PUBLIC_IP 1194
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-CBC
auth SHA256
verb 3

# Paste contents of these files from server:
# ca.crt
# client.crt
# client.key
# ta.key
```

**Pros:**
- Full control over your VPN
- No third-party dependencies
- Can be hosted on any VPS

**Cons:**
- Requires port forwarding on router/firewall
- APK clients would need OpenVPN installed (impractical for most Android devices)

---

#### WireGuard (Modern VPN)

**Why not recommended for L3MON:** APK clients would need WireGuard installed, which is impractical.

---

### Reverse Tunneling & Port Forwarding Services

These services create a public URL that tunnels to your local L3MON server.

#### Ngrok

**What it is:** Creates a public URL that tunnels to your localhost. Free tier available.

**Setup:**

```bash
# Download Ngrok
cd /tmp
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
tar -xzf ngrok-v3-stable-linux-amd64.tgz
sudo mv ngrok /usr/local/bin/
rm ngrok-v3-stable-linux-amd64.tgz

# Create free account at https://ngrok.com
# Get your authtoken from the dashboard

# Configure Ngrok
ngrok config add-authtoken YOUR_AUTH_TOKEN

# Start Ngrok tunnel for L3MON Dashboard
ngrok http 22533

# For L3MON client connections, you need TCP:
ngrok tcp 22222
```

**Ngrok Configuration File:**

```bash
nano ~/.ngrok3/ngrok.yml
```

```yaml
version: "2"
authtoken: YOUR_AUTH_TOKEN

tunnels:
  l3mon-dashboard:
    addr: 22533
    proto: http
    bind-tls: true
  
  l3mon-client:
    addr: 22222
    proto: tcp
```

```bash
# Start with config
ngrok start --all
# OR
ngrok start l3mon-dashboard l3mon-client
```

**⚠️ IMPORTANT FOR L3MON:**
When building the APK, you need:
- **LHOST:** The TCP tunnel URL from Ngrok (e.g., `0.tcp.ngrok.io:12345`)
- **LPORT:** The port shown in the TCP tunnel

**Pros:**
- Easy setup
- No router configuration needed
- Works behind NAT
- Free tier available

**Cons:**
- Free tier has limitations
- URLs change on reconnect
- Third-party service
- Connection speed depends on Ngrok servers

---

#### Cloudflare Tunnel (frp / bore)

Similar to Ngrok, these create public tunnels to your local server:

**bore:**
```bash
# Install bore
cargo install bore-cli
# OR
wget https://github.com/ekzhang/bore/releases/download/v0.5.0/bore-v0.5.0-x86_64-unknown-linux-musl.tar.gz
tar -xzf bore-v0.5.0-x86_64-unknown-linux-musl.tar.gz
sudo mv bore /usr/local/bin/

# Start tunnel
bore local 22533 --to bore.pub
bore local 22222 --to bore.pub
```

**localtunnel:**
```bash
# Install via npm
npm install -g localtunnel

# Start tunnel
lt --port 22533
lt --port 22222 --subdomain my-l3mon
```

---

### Cloud VPS Setup (AWS)

Using a cloud VPS gives you a persistent public IP and is the most reliable solution for L3MON.

#### AWS EC2 Setup

##### Step 1: Create AWS Account

1. Go to [https://aws.amazon.com/](https://aws.amazon.com/)
2. Click "Create an AWS Account"
3. Complete the registration (credit card required)

##### Step 2: Launch an EC2 Instance

1. Log in to AWS Console
2. Go to **EC2 Dashboard**
3. Click **Launch Instance**

**Configure Instance:**
```
Name: Kali-L3MON
AMI: Ubuntu Server 22.04 LTS (Free tier eligible)
Instance Type: t2.micro (Free tier) or t3.small (better performance)
Key Pair: Create new (download .pem file)
Network Settings:
  - VPC: Default
  - Subnet: Default
  - Auto-assign public IP: Enable
  - Firewall (Security Group): Create new
  
Security Group Rules:
  - SSH (2222): My IP
  - HTTP (80): Anywhere
  - HTTPS (443): Anywhere
  - Custom TCP (22533): Anywhere
  - Custom TCP (22222): Anywhere
```

4. Click **Launch Instance**

##### Step 3: Connect to Instance

```bash
# Change permissions for key file
chmod 400 kali-l3mon.pem

# Connect via SSH
ssh -i kali-l3mon.pem -p 2222 ubuntu@YOUR_PUBLIC_IP
```

##### Step 4: Install Dependencies

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install L3MON dependencies
sudo apt install -y openjdk-8-jdk openjdk-8-jre nodejs npm git curl wget

# Install apktool
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh)

# Install PM2
sudo npm install pm2 -g

# Clone and setup L3MON
cd ~
git clone https://github.com/efxtv/L3MON.git
cd L3MON
npm install
npm audit fix --force
```

##### Step 5: Configure Firewall (AWS Security Groups)

**In AWS Console:**
1. EC2 → Instances → Select your instance
2. Click the Security tab
3. Click the Security Group link
4. Edit Inbound Rules:

```
Type: SSH | Port: 2222 | Source: My IP
Type: HTTP | Port: 80 | Source: Anywhere
Type: HTTPS | Port: 443 | Source: Anywhere
Type: Custom TCP | Port: 22533 | Source: Anywhere
Type: Custom TCP | Port: 22222 | Source: Anywhere
```

##### Step 6: Install and Configure UFW on EC2

```bash
# Install UFW
sudo apt install -y ufw

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH on custom port
sudo ufw allow 2222/tcp

# Allow L3MON ports
sudo ufw allow 22533/tcp
sudo ufw allow 22222/tcp

# Enable UFW
sudo ufw enable
```

##### Step 7: Start L3MON

```bash
cd ~/L3MON

# Start with PM2
pm2 start index.js --name l3mon

# Configure admin password
nano maindb.json
# Edit admin username and password (MD5 hash)

# Save and restart
pm2 restart l3mon
pm2 save
pm2 startup
```

##### Step 8: Access L3MON

```
Dashboard: http://YOUR_PUBLIC_IP:22533
```

**⚠️ IMPORTANT:** When building the APK:
- **LHOST:** Your EC2 public IP
- **LPORT:** 22222

---

#### Other Cloud Providers

**DigitalOcean:**
- Similar process to AWS
- Cheaper starting prices ($4/month)
- Easier interface
- Tutorial: Create Droplet → Ubuntu 22.04 → Follow L3MON install steps

**Linode:**
- Good performance
- $5/month starting
- Same process as AWS

**Vultr:**
- Global server locations
- $2.50/month for smallest instance
- Fast SSD storage

**Oracle Cloud (Free):**
- Always Free tier with 2 VMs
- No cost forever (if within limits)
- Good for testing

---

### Comparison: Network Solutions for L3MON

| Solution | Cost | Difficulty | Reliability | Best For |
|----------|------|------------|-------------|----------|
| **Bridged + Local Network** | Free | Easy | Good | Local testing only |
| **Ngrok** | Free/Low | Easy | Medium | Quick testing, demos |
| **Port Forwarding (Router)** | Free | Medium | High | Home/Office setups |
| **OpenVPN on VPS** | $$ | Hard | Very High | Full control, production |
| **AWS EC2** | Low/Free | Medium | Very High | Production, persistent |
| **DigitalOcean** | $ | Easy | Very High | Production, easy setup |

---

### Router Port Forwarding (Home Network)

If running L3MON at home, configure your router:

**Steps:**

1. **Find your router's IP:**
```bash
ip route | grep default
# or
ipconfig (Windows)
```

2. **Access Router Admin Panel:**
```
Open browser: http://192.168.1.1 (or your router IP)
Login with admin credentials (usually on router)
```

3. **Find Port Forwarding Section:**
- Often under: Advanced → NAT → Port Forwarding
- Or: Firewall → Virtual Servers
- Or: Applications → Port Forwarding

4. **Add Forwarding Rules:**
```
Service Name: L3MON-Dashboard
External Port: 22533
Internal IP: Your VM/Kali IP (e.g., 192.168.1.100)
Internal Port: 22533
Protocol: TCP

Service Name: L3MON-Client
External Port: 22222
Internal IP: Your VM/Kali IP (e.g., 192.168.1.100)
Internal Port: 22222
Protocol: TCP
```

5. **Save and Restart Router (if needed)**

**⚠️ Note:** Your public IP changes periodically (dynamic IP). Consider using a dynamic DNS service.

---

### Dynamic DNS (DDNS)

Since home IPs change, use DDNS to get a permanent domain:

#### No-IP (Free)

```bash
# Register at https://www.noip.com
# Create a hostname (e.g., myl3mon.ddns.net)

# Install No-IP client on Kali
sudo apt install -y noip2

# Configure
sudo noip2 -C
# Enter your credentials and hostname

# Start
sudo noip2
```

#### Alternative: DuckDNS (Free)

```bash
# Register at https://www.duckdns.org
# Create subdomain

# Update via cron
echo "*/5 * * * * curl 'https://www.duckdns.org/update?domains=YOUR_DOMAIN&token=YOUR_TOKEN'" >> /etc/crontab
```

---

### Testing Your L3MON Setup

**From Internal Network:**
```bash
# Test dashboard
curl http://localhost:22533
# or
curl http://192.168.1.100:22533

# Test from external network (phone on mobile data)
# Open browser to http://YOUR_PUBLIC_IP:22533
```

**Testing APK Connection:**
```bash
# From external machine, test port is open
nc -zv YOUR_PUBLIC_IP 22222
# or
telnet YOUR_PUBLIC_IP 22222
```

**Using Ngrok (Quick Test):**
```bash
# Terminal 1: Start L3MON
cd ~/L3MON
pm2 start index.js

# Terminal 2: Start Ngrok
ngrok tcp 22222
# Note the forwarding URL (e.g., 0.tcp.ngrok.io:12345)

# Build APK with:
# LHOST: 0.tcp.ngrok.io
# LPORT: 12345
```

---

### Network Security Checklist

Before exposing L3MON to the internet:

- [ ] Change default SSH port (22 → 2222)
- [ ] Disable password authentication for SSH (use keys only)
- [ ] Install and configure Fail2Ban
- [ ] Configure UFW/iptables firewall
- [ ] Allow only necessary ports (22533, 22222)
- [ ] Use HTTPS/SSL for dashboard (Nginx reverse proxy)
- [ ] Enable DDoS protection (CloudFlare, etc.)
- [ ] Regular security updates: `sudo apt update && sudo apt upgrade -y`
- [ ] Monitor logs regularly
- [ ] Backup configuration files

---

### Troubleshooting Network Issues

**Can't access from external network:**
```bash
# Check if services are running
pm2 status
sudo netstat -tulpn | grep -E '22533|22222'

# Check firewall
sudo ufw status
sudo iptables -L -n | grep -E '22533|22222'

# Test local connectivity
curl http://127.0.0.1:22533
telnet localhost 22533

# Check router port forwarding
# Use online port checker: https://canyouseeme.org/
```

**DNS/hostname issues:**
```bash
# Check if DNS resolves
nslookup myl3mon.ddns.net
dig myl3mon.ddns.net

# Test IP connectivity
ping 8.8.8.8
traceroute 8.8.8.8
```

**Slow connections:**
```bash
# Check network speed
speedtest-cli

# Check for packet loss
ping -c 100 8.8.8.8

# Monitor bandwidth
sudo apt install -y nethogs
sudo nethogs
```

---



### Java Version Issues

```bash
# Check all Java versions
sudo update-alternatives --list java

# Check current Java
which java
java -version

# Check JAVA_HOME
echo $JAVA_HOME

# Set Java 8 as default
sudo update-alternatives --config java
sudo update-alternatives --config javac

# Verify
java -version
javac -version

# If Java is not found, re-add it:
sudo update-alternatives --install "/usr/bin/java" "java" "/opt/jdk8u432-b06/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/opt/jdk8u432-b06/bin/javac" 1
```

### Node.js Issues

```bash
# Check Node version
node --version

# Clean npm cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### PM2 Issues

```bash
# Clear PM2
pm2 delete all
pm2 flush

# Restart L3MON
pm2 start index.js --name l3mon
pm2 logs l3mon
```

### APK Build Issues

```bash
# Reinstall apktool
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh)

# Check apktool version
apktool

# Manual apktool installation
cd /tmp
wget https://github.com/iBotPeaches/Apktool/releases/download/v2.8.1/apktool_2.8.1.jar
mv apktool_2.8.1.jar apktool.jar
sudo mv apktool.jar /usr/local/bin/
sudo chmod +x /usr/local/bin/apktool.jar
echo 'function apktool() { java -jar /usr/local/bin/apktool.jar "$@"; }' >> ~/.bashrc
source ~/.bashrc
```

### Port Already in Use

```bash
# Check what's using port 22533
sudo lsof -i :22533
sudo netstat -tulpn | grep 22533

# Kill the process if needed
sudo kill $(sudo lsof -t -i:22533)
```

### Docker Issues

```bash
# Restart Docker
sudo systemctl restart docker

# Check Docker logs
sudo journalctl -u docker -f

# Remove and recreate container
sudo docker stop l3mon
sudo docker rm l3mon
sudo docker rmi l3mon:latest
```

---

## Uninstall/Cleanup

### Remove L3MON (Native)

```bash
# Stop PM2
pm2 stop l3mon
pm2 delete l3mon
pm2 save

# Remove directory
cd ~
rm -rf ~/tools/L3MON
```

### Remove Docker L3MON

```bash
# Stop and remove container
sudo docker stop l3mon
sudo docker rm l3mon

# Remove image
sudo docker rmi l3mon:latest

# Remove Docker if needed
sudo apt purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
```

### Remove Java 8 (If Needed)

```bash
# Remove Java alternatives
sudo update-alternatives --remove java /opt/jdk8u432-b06/bin/java
sudo update-alternatives --remove javac /opt/jdk8u432-b06/bin/javac
sudo update-alternatives --remove javaws /opt/jdk8u432-b06/bin/javaws

# Remove Java directory
sudo rm -rf /opt/jdk8u432-b06

# Remove environment files
sudo rm /etc/profile.d/java8.sh
sed -i '/JAVA_HOME/d' ~/.bashrc
sed -i '/jdk8/d' ~/.bashrc
```

### Full System Cleanup

```bash
# Remove Java 8
sudo update-alternatives --remove java /opt/jdk8u432-b06/bin/java 2>/dev/null
sudo update-alternatives --remove javac /opt/jdk8u432-b06/bin/javac 2>/dev/null
sudo rm -rf /opt/jdk8u432-b06
sudo rm /etc/profile.d/java8.sh

# Remove all tools and dependencies
sudo apt remove nodejs npm ufw fail2ban nginx
sudo apt autoremove -y
sudo apt autoclean

# Reset UFW
sudo ufw reset

# Clean PM2
npm remove pm2 -g
```

---

## Quick Reference Commands

### Essential Commands

| Task | Command |
|------|---------|
| Start L3MON | `pm2 start index.js --name l3mon` |
| Stop L3MON | `pm2 stop l3mon` |
| Restart L3MON | `pm2 restart l3mon` |
| View Logs | `pm2 logs l3mon` |
| Check Status | `pm2 status` |
| Enable UFW | `sudo ufw enable` |
| Check UFW Status | `sudo ufw status verbose` |
| Open Port | `sudo ufw allow PORT/tcp` |
| Block Port | `sudo ufw deny PORT/tcp` |
| View Logs | `tail -f /var/log/l3mon.log` |

### Important Ports

| Port | Service | Purpose |
|------|---------|---------|
| 22 | SSH | Default SSH (should be blocked/disabled) |
| 22533 | L3MON | L3MON RAT Dashboard (Web Interface) |
| 22222 | L3MON | L3MON Connection Port (Android APK clients) |
| 2222 | SSH | Custom SSH port (secure) |
| 80/443 | HTTP/HTTPS | Web server / Reverse Proxy |

### Networking Quick Reference

| Task | Command |
|------|---------|
| Check network interfaces | `ip a` |
| Test port connectivity | `nc -zv IP PORT` |
| Check listening ports | `ss -tulpn` |
| View routing table | `ip route` |
| Test DNS resolution | `nslookup domain.com` |
| Speed test | `speedtest-cli` |

---

## Legal Disclaimer

> **This guide is for educational purposes only. The tools and techniques described should only be used on:
> - Devices you own
> - Devices you have explicit written authorization to test
>
> **Unauthorized access to computer systems is illegal and can result in criminal charges.** The author takes no responsibility for misuse of this information.

---

## References

- [L3MON GitHub Repository](https://github.com/efxtv/L3MON)
- [Kali Linux Official Documentation](https://www.kali.org/docs/)
- [VirtualBox Documentation](https://www.virtualbox.org/wiki/Documentation)
- [VMware Documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/index.html)
- [Docker Documentation](https://docs.docker.com/)
- [UFW Documentation](https://help.ubuntu.com/community/UFW)
- [Ngrok Documentation](https://ngrok.com/docs)
- [OpenVPN Documentation](https://openvpn.net/community-resources/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [WireGuard Documentation](https://www.wireguard.com/quickstart/)
- [DigitalOcean Tutorials](https://www.digitalocean.com/community/tutorials)
- [Fail2Ban Documentation](https://www.fail2ban.org/wiki/index.php/Main_Page)

---

**Last Updated: July 2026**
