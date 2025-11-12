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
