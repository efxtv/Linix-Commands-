### Linux File System Hierarchy.

---

# 🧭 Essential Linux File System Structure (Ubuntu/Fedora)

When you install Ubuntu or any Linux distribution, you get a structured filesystem that looks different from Windows.
There are no “C:” or “D:” drives — everything starts from a single root directory `/`.

Understanding this hierarchy helps you know **where system files, user files, and programs live**.

---

### 1. `/` — Root Directory

This is the **top-most directory** in Linux.
Everything (files, folders, and devices) exists inside it.
It’s the base of the Linux filesystem tree.

Example structure:

```
/
├── bin
├── etc
├── home
├── usr
└── var
```

You can view its contents with:

```bash
ls /
```

---

### 2. `/bin` — Essential User Commands

Contains **fundamental command binaries** used by all users.
These are the basic tools required to operate the system.

Common programs stored here:
`ls`, `cp`, `mv`, `rm`, `cat`, `pwd`, `mkdir`, `echo`

View them with:

```bash
ls /bin
```

---

### 3. `/sbin` — System Administration Commands

Contains **system administration tools** used mostly by the root (admin) user.
Used for disk management, network setup, and maintenance.

Examples:
`fdisk`, `reboot`, `ifconfig`, `shutdown`, `mkfs`

View them with:

```bash
ls /sbin
```

---

### 4. `/etc` — Configuration Files

Holds **system-wide configuration files** for software and services.
Most are plain text and editable (with `sudo`).

Examples:

* `/etc/passwd` — user accounts
* `/etc/hostname` — system name
* `/etc/fstab` — disk mount info

To explore:

```bash
ls /etc
```

---

### 5. `/home` — User Home Directories

Each user has a personal folder here for documents, downloads, and settings.
For example:

```
/home/student
/home/teacher
```

To check your own home directory:

```bash
cd ~
pwd
```

This is similar to `C:\Users\username` in Windows.

---

### 6. `/var` — Variable Data Files

Used for files that change frequently, like logs, caches, and databases.

Common subfolders:

* `/var/log` → system and app logs
* `/var/cache` → cached data
* `/var/spool` → print/mail queues
* `/var/lib` → databases

To view logs:

```bash
ls /var/log
```

---

### 7. `/tmp` — Temporary Files

Stores **temporary data** used by applications or processes.
Files here are deleted automatically on reboot.

To check:

```bash
ls /tmp
```

Do not save important data here — it’s wiped periodically.

---

### 8. `/usr` — User Programs and Libraries

Contains **user-installed programs, libraries, and documentation**.

Subdirectories:

* `/usr/bin` → general programs (nano, vim, python)
* `/usr/sbin` → admin tools
* `/usr/lib` → libraries
* `/usr/share` → shared files (icons, docs)

View user programs:

```bash
ls /usr/bin
```

---

### 9. `/lib` — System Libraries

Contains **essential shared libraries** used by `/bin` and `/sbin` programs.
They are like `.dll` files in Windows.

Example location:

```
/lib/x86_64-linux-gnu/
```

Check them with:

```bash
ls /lib
```

---

### 10. `/opt` — Optional Software

Used for **third-party or manually installed software**.
For example:

```
/opt/google/chrome
/opt/visual-studio-code
```

To check:

```bash
ls /opt
```

This is similar to “Program Files” on Windows.

---

### 11. `/root` — Root User’s Home

The personal folder of the **root (administrator)** user.
Different from `/` (the system root).

Path:

```
/root
```

Switch to root home:

```bash
sudo -i
cd /root
```

---

### 12. `/boot` — Boot Files

Contains everything required to **start (boot)** Linux.

Important files:

* `/boot/vmlinuz` → Linux kernel
* `/boot/initrd.img` → temporary file system
* `/boot/grub/` → GRUB bootloader config

View boot contents:

```bash
ls /boot
```

Do not delete or modify anything here!

---

### 13. `/dev` — Device Files

Linux treats **hardware as files**, and they live in `/dev`.

Examples:

* `/dev/sda` → hard drive
* `/dev/sdb` → USB drive
* `/dev/null` → “black hole” for discarded output

List connected devices:

```bash
ls /dev
```

---

### 14. `/proc` — Process Information

A **virtual filesystem** that shows live system and process details.
It doesn’t exist on disk — created dynamically by the kernel.

Examples:

* `/proc/cpuinfo` → CPU details
* `/proc/meminfo` → memory info
* `/proc/1234/` → details of process ID 1234

To view CPU info:

```bash
cat /proc/cpuinfo
```

---

### 15. `/sys` — System and Hardware Info

Another virtual directory for hardware and driver data.
Used by the kernel to manage connected devices.

Examples:

* `/sys/class/net/` → network interfaces
* `/sys/block/` → disks and storage

View network interfaces:

```bash
ls /sys/class/net
```

---

### Summary Table

| Directory | Description         | Example Contents   |
| --------- | ------------------- | ------------------ |
| `/`       | Root of the system  | All other folders  |
| `/bin`    | Basic commands      | `ls`, `rm`, `cp`   |
| `/sbin`   | Admin commands      | `fdisk`, `reboot`  |
| `/etc`    | System configs      | `passwd`, `fstab`  |
| `/home`   | User data           | `/home/student`    |
| `/var`    | Variable files      | `/var/log/syslog`  |
| `/tmp`    | Temporary files     | `/tmp/file`        |
| `/usr`    | User programs       | `nano`, `python`   |
| `/lib`    | System libraries    | `.so` files        |
| `/opt`    | Optional software   | Chrome, VSCode     |
| `/root`   | Root’s home         | Private admin data |
| `/boot`   | Bootloader & kernel | GRUB configs       |
| `/dev`    | Devices             | `/dev/sda`         |
| `/proc`   | Process info        | `/proc/cpuinfo`    |
| `/sys`    | Hardware info       | `/sys/class/net`   |

---

### Teaching Tips for Beginners

1. Show the root structure:

   ```bash
   ls /
   ```
2. Explore `/etc` safely:

   ```bash
   ls /etc
   ```
3. Open system logs:

   ```bash
   less /var/log/syslog
   ```
4. Check CPU and memory info:

   ```bash
   cat /proc/cpuinfo
   cat /proc/meminfo
   ```
5. Visit the user’s home directory:

   ```bash
   cd ~
   ls -a
   ```

---

### Final Advice for Students

* Explore directories using `cd` and `ls`, but avoid editing system folders without `sudo`.
* `/etc`, `/boot`, and `/dev` are critical — handle with care.
* Most personal work happens in `/home/username`.

---

**Maintained by:** [EFXTv]

**License:** MIT

**Last Updated:** November 2025
---
