---
                                                           Linux A2Z tutorial
---

# 🖥️ Top 50 Linux Commands for Beginners

### **1. File & Directory Management**

| Command | Description                        |
| ------- | ---------------------------------- |
| `pwd`   | Show current directory             |
| `ls`    | List files and directories         |
| `cd`    | Change directory                   |
| `mkdir` | Create a directory                 |
| `rmdir` | Remove empty directory             |
| `rm`    | Remove file/directory              |
| `cp`    | Copy file/directory                |
| `mv`    | Move/rename file/directory         |
| `touch` | Create empty file                  |
| `tree`  | Show directory structure as a tree |
| `find`  | Search files by name, type, size   |

---

### **2. Viewing & Editing Files**

| Command      | Description                        |
| ------------ | ---------------------------------- |
| `cat`        | View file content                  |
| `tac`        | View file content in reverse       |
| `less`       | Scroll through file content        |
| `more`       | View file page by page             |
| `head`       | View first lines of a file         |
| `tail`       | View last lines of a file          |
| `nano`       | Simple text editor                 |
| `vi` / `vim` | Advanced text editor               |
| `grep`       | Search for text inside files       |
| `wc`         | Count lines, words, and characters |
| `diff`       | Compare two files                  |

---

### **3. Permissions & Ownership**

| Command | Description                           |
| ------- | ------------------------------------- |
| `chmod` | Change file/directory permissions     |
| `chown` | Change file owner                     |
| `chgrp` | Change file group                     |
| `umask` | Set default permissions for new files |
| `ls -l` | Show detailed list with permissions   |

---

### **4. System & Hardware Info**

| Command    | Description                                |
| ---------- | ------------------------------------------ |
| `uname`    | Show system info                           |
| `uname -r` | Show kernel version                        |
| `top`      | Show running processes                     |
| `htop`     | Interactive process viewer (needs install) |
| `df`       | Show disk space usage                      |
| `du`       | Show directory/file size                   |
| `free`     | Show RAM usage                             |
| `uptime`   | Show system uptime and load                |
| `who`      | Show logged in users                       |
| `w`        | Show active users and processes            |
| `id`       | Show user ID and groups                    |
| `hostname` | Show system hostname                       |
| `lscpu`    | CPU information                            |
| `lsblk`    | Block device information                   |
| `lspci`    | PCI devices                                |
| `lsusb`    | USB devices                                |

---

### **5. Package Management**

*(Depends on distro)*
**Ubuntu/Debian:** `apt`
**Fedora/RedHat:** `dnf`

| Command                                   | Description                |
| ----------------------------------------- | -------------------------- |
| `apt update` / `dnf check-update`         | Update package list        |
| `apt upgrade` / `dnf upgrade`             | Upgrade installed packages |
| `apt install <pkg>` / `dnf install <pkg>` | Install software           |
| `apt remove <pkg>` / `dnf remove <pkg>`   | Remove software            |
| `apt search <pkg>` / `dnf search <pkg>`   | Search software            |

---

### **6. Networking**

| Command             | Description                       |
| ------------------- | --------------------------------- |
| `ping`              | Test connectivity to a host       |
| `ifconfig` / `ip a` | Show network interfaces           |
| `curl`              | Fetch a URL / test HTTP requests  |
| `wget`              | Download files from internet      |
| `ssh`               | Connect to remote machine         |
| `scp`               | Copy files to/from remote machine |
| `netstat` / `ss`    | Show network connections          |
| `traceroute`        | Show path to a host               |

---

### **7. Miscellaneous**

| Command   | Description                   |
| --------- | ----------------------------- |
| `echo`    | Print text to terminal        |
| `date`    | Show current date/time        |
| `cal`     | Show calendar                 |
| `history` | Show command history          |
| `alias`   | Create shortcuts for commands |
| `clear`   | Clear terminal screen         |
| `man`     | Open manual for a command     |
| `sudo`    | Run commands as root          |

---

### ✅ Tips for Beginners

1. Use **`man <command>`** to learn more about each command.
2. Experiment in a **safe directory**, e.g., `/home/user/test`.
3. Combine commands with **pipes (`|`)** and **redirection (`>` / `>>`)** as you get comfortable.
4. Start with file navigation, then move to system info, then networking and package management.

---

#------------------------------------------------------------------------------ EXTRAS

---

# 🔹 Netstat & SS Tutorial

---

## 1️⃣ `netstat` (Network Statistics)

`netstat` is a classic command for displaying **network connections, routing tables, interface stats, etc.**

### **Basic Usage**

```bash
netstat
```

* Shows **all active connections** on your system (TCP, UDP, UNIX sockets, etc.)

---

### **Common Options**

| Option | Description                                           |
| ------ | ----------------------------------------------------- |
| `-a`   | Show **all connections**, listening and non-listening |
| `-t`   | Show only **TCP connections**                         |
| `-u`   | Show only **UDP connections**                         |
| `-l`   | Show only **listening ports**                         |
| `-n`   | Show **numeric addresses/ports** instead of hostnames |
| `-p`   | Show **process ID and name** using the connection     |
| `-r`   | Show **routing table**                                |
| `-i`   | Show **network interface stats**                      |

---

### **Example: Show all listening TCP ports**

```bash
netstat -tln
```

**Explanation:**

* `-t` → TCP only
* `-l` → listening ports
* `-n` → numeric output

**Sample Output:**

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp6       0      0 :::80                   :::*                    LISTEN
```

**Fields:**

* `Proto` → Protocol (TCP/UDP)
* `Recv-Q` / `Send-Q` → Queue sizes for incoming/outgoing data
* `Local Address` → IP and port on your machine
* `Foreign Address` → Remote IP and port
* `State` → Status (LISTEN, ESTABLISHED, TIME_WAIT, etc.)

---

### **Show process using ports**

```bash
sudo netstat -tulpn
```

* `-t` TCP, `-u` UDP
* `-l` Listening
* `-p` Show PID/program name
* `-n` Numeric

---

### ⚠️ Note:

* On **modern Linux**, `netstat` is **deprecated** in favor of `ss`.
* `net-tools` package is needed for `netstat` on some distributions.

---

## 2️⃣ `ss` (Socket Statistics)

`ss` is **faster and more powerful** than `netstat`. It can display **detailed socket info** quickly.

### **Basic Usage**

```bash
ss
```

* Shows **all active connections**.

---

### **Common Options**

| Option | Description                             |
| ------ | --------------------------------------- |
| `-t`   | TCP connections                         |
| `-u`   | UDP connections                         |
| `-l`   | Listening sockets only                  |
| `-n`   | Show numeric addresses/ports            |
| `-p`   | Show process using socket               |
| `-a`   | All sockets (listening + non-listening) |
| `-s`   | Summary of socket statistics            |

---

### **Example: Show all listening TCP ports**

```bash
ss -tln
```

**Sample Output:**

```
State      Recv-Q Send-Q Local Address:Port  Peer Address:Port
LISTEN     0      128   0.0.0.0:22         0.0.0.0:*
LISTEN     0      128   [::]:80            [::]:*
```

**Fields:**

* `State` → Socket state (LISTEN, ESTAB, TIME-WAIT, etc.)
* `Recv-Q` / `Send-Q` → Queue sizes
* `Local Address:Port` → Your machine’s IP and port
* `Peer Address:Port` → Remote machine’s IP and port

---

### **Show process using ports**

```bash
sudo ss -tulpn
```

* Works exactly like `netstat -tulpn`
* Shows **PID and program name** listening or connected

---

### **Summary: netstat vs ss**

| Feature                     | `netstat`              | `ss`                   |
| --------------------------- | ---------------------- | ---------------------- |
| Speed                       | Slower                 | Faster                 |
| Modern Linux                | Deprecated             | Recommended            |
| Output                      | Standard, older format | Cleaner, more detailed |
| Example for listening ports | `netstat -tln`         | `ss -tln`              |

---

### **Practical Tips**

1. Use `ss -tulpn` to check **which process is using a port**.
2. Use `ss -s` to **get socket summary** for debugging network issues.
3. Combine with `grep` to filter, e.g.,

   ```bash
   ss -tulpn | grep :22
   ```

   → Check if SSH is running on port 22

---

