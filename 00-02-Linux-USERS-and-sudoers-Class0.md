
# Linux User Creation and Management Commands

This document provides a detailed reference for creating, managing, and deleting users in Linux.

---

## 1. `useradd`

**Description:**  
`useradd` is used to create a new user account. It does not set a password by default; you need to use `passwd` afterward.

**Syntax:**  
```bash
useradd [options] username
```

**Important Options:**

| Option              | Description                                                          |
| ------------------- | -------------------------------------------------------------------- |
| `-c "comment"`      | Sets a description or full name (stored in GECOS field).             |
| `-d /home/username` | Specify a custom home directory.                                     |
| `-m`                | Create the user's home directory if it does not exist.               |
| `-M`                | Do not create home directory.                                        |
| `-s /bin/shell`     | Set the login shell (default: `/bin/sh`).                            |
| `-e YYYY-MM-DD`     | Account expiration date.                                             |
| `-f days`           | Number of days after password expiration before account is disabled. |
| `-u UID`            | Specify a custom user ID (UID).                                      |
| `-g group`          | Primary group for the user. Must exist.                              |
| `-G group1,group2`  | Comma-separated list of supplementary groups.                        |
| `-r`                | Create a system account (UID < 1000).                                |
| `-K KEY=VALUE`      | Override defaults from `/etc/login.defs`.                            |

**Example:**

```bash
# Create a user 'johndoe' with home directory, bash shell, and comment
sudo useradd -m -s /bin/bash -c "John Doe" johndoe
```

**Notes:**

* Default settings are defined in `/etc/default/useradd`.
* The user entry is added to `/etc/passwd`.
* The user’s password must be set using `passwd`.

---

## 2. `passwd`

**Description:**
Sets or changes a user’s password. Can also lock or unlock an account.

**Syntax:**

```bash
passwd [options] username
```

**Options:**

| Option         | Description                                                 |
| -------------- | ----------------------------------------------------------- |
| `-l`           | Lock the password (account still exists but cannot log in). |
| `-u`           | Unlock the password.                                        |
| `-d`           | Delete the password (account becomes passwordless).         |
| `-e`           | Force password expiration (user must change on next login). |
| `-n MIN_DAYS`  | Set minimum days between password changes.                  |
| `-x MAX_DAYS`  | Set maximum days a password is valid.                       |
| `-w WARN_DAYS` | Days of warning before password expiration.                 |

**Example:**

```bash
# Set password for johndoe
sudo passwd johndoe

# Force johndoe to change password at next login
sudo passwd -e johndoe
```

---

## 3. `usermod`

**Description:**
Modifies an existing user account. Useful for changing home directories, shells, groups, or usernames.

**Syntax:**

```bash
usermod [options] username
```

**Options:**

| Option              | Description                                     |
| ------------------- | ----------------------------------------------- |
| `-l newname`        | Change username.                                |
| `-d /new/home -m`   | Change home directory and move contents.        |
| `-s /bin/shell`     | Change login shell.                             |
| `-c "comment"`      | Update the GECOS field.                         |
| `-g group`          | Change primary group.                           |
| `-G group1,group2`  | Set supplementary groups (overwrites existing). |
| `-aG group1,group2` | Append user to supplementary groups.            |
| `-L`                | Lock the account.                               |
| `-U`                | Unlock the account.                             |
| `-e YYYY-MM-DD`     | Set account expiration date.                    |

**Example:**

```bash
# Add johndoe to sudo and docker groups without removing existing groups
sudo usermod -aG sudo,docker johndoe

# Change johndoe's home directory
sudo usermod -d /home/johndoe_new -m johndoe
```

---

## 4. `userdel`

**Description:**
Deletes a user account. Can also remove home directory and mail spool.

**Syntax:**

```bash
userdel [options] username
```

**Options:**

| Option | Description                                                                    |
| ------ | ------------------------------------------------------------------------------ |
| `-r`   | Remove home directory and mail spool.                                          |
| `-f`   | Force deletion even if user is logged in or has running processes (dangerous). |

**Example:**

```bash
# Delete johndoe and remove home directory
sudo userdel -r johndoe
```

---

## 5. `id`

**Description:**
Displays a user’s UID, primary GID, and supplementary groups.

**Syntax:**

```bash
id [username]
```

**Example:**

```bash
id johndoe
# Output: uid=1001(johndoe) gid=1001(johndoe) groups=1001(johndoe),27(sudo)
```

---

## 6. `groups`

**Description:**
Shows groups the specified user belongs to.

**Syntax:**

```bash
groups [username]
```

**Example:**

```bash
groups johndoe
# Output: johndoe : johndoe sudo docker
```

---

## 7. `chage`

**Description:**
Manages password aging policies.

**Syntax:**

```bash
chage [options] username
```

**Options:**

| Option          | Description                                                                |
| --------------- | -------------------------------------------------------------------------- |
| `-l`            | List current aging information.                                            |
| `-m days`       | Minimum number of days between password changes.                           |
| `-M days`       | Maximum number of days a password is valid.                                |
| `-W days`       | Number of days before password expiration to warn the user.                |
| `-I days`       | Number of inactive days after password expires before account is disabled. |
| `-E YYYY-MM-DD` | Set account expiration date.                                               |

**Example:**

```bash
# Set johndoe's password to expire after 90 days with a 14-day warning
sudo chage -M 90 -W 14 johndoe
```

---

## 8. `getent passwd`

**Description:**
Fetches user information from system databases (`/etc/passwd`, LDAP, NIS).

**Syntax:**

```bash
getent passwd [username]
```

**Example:**

```bash
getent passwd johndoe
# Output: johndoe:x:1001:1001:John Doe:/home/johndoe:/bin/bash
```

---

## 9. `finger` (optional)

**Description:**
Shows detailed info about a user: home directory, shell, last login.

**Syntax:**

```bash
finger username
```

**Example:**

```bash
finger johndoe
```

---

## 10. `/etc/passwd` and `/etc/shadow`

* `/etc/passwd`: Stores user account info (username, UID, GID, comment, home, shell).
* `/etc/shadow`: Stores encrypted passwords and aging information. Only readable by root.

**Example entry in `/etc/passwd`:**

```
johndoe:x:1001:1001:John Doe:/home/johndoe:/bin/bash
```

**Example entry in `/etc/shadow`:**

```
johndoe:$6$abcd1234$...:19336:0:99999:7:::
```

---

## 11. Best Practices

* Always use `-m` when creating users to ensure a home directory is created.
* Use `-s /bin/bash` or another valid shell; some commands default to `/bin/sh`.
* Avoid using `userdel -f` unless necessary; it can disrupt processes.
* Regularly review `/etc/passwd`, `/etc/shadow`, and group memberships.
* Use `chage` to enforce security policies on password expiration.

---

This file serves as a complete reference for **user creation, modification, deletion, and security management** on Linux systems.

```

If you want, I can make an **even more exhaustive version** that also includes:

- **Group management commands** (`groupadd`, `groupdel`, `gpasswd`)  
- **System vs regular users**  
- **UID/GID ranges and best practices**  
- **Automating user creation via scripts**  
```


---
                                        SUDO AND SUDOERS FILE
---

Absolutely! Here's a **comprehensive GitHub Markdown post** about the `sudoers` file and `sudo` command, fully detailed for learning or documentation purposes.


# Understanding `sudoers` and the `sudo` Command in Linux

The `sudo` command allows a permitted user to execute a command as the superuser or another user, as specified by the security policy. The configuration that controls `sudo` permissions is stored in the `sudoers` file.

---

## 1. What is `sudo`?

- **sudo** stands for **"superuser do"**.
- It allows regular users to run commands with elevated privileges (root), without needing the root password.
- Provides an **audit trail** of commands run with elevated privileges.

**Syntax:**
```bash
sudo [options] command
````

**Example:**

```bash
# Update package lists (requires root privileges)
sudo apt update
```

---

## 2. The `sudoers` File

* Located at `/etc/sudoers`.
* Defines which users or groups can run commands with `sudo`.
* Must be edited with the `visudo` command to prevent syntax errors.

**Do not edit `/etc/sudoers` directly**; always use:

```bash
sudo visudo
```

---

## 3. Basic Syntax of `sudoers`

Each line in `sudoers` has this general form:

```
user  host = (runas) command
```

| Field     | Description                                            |
| --------- | ------------------------------------------------------ |
| `user`    | The username or group allowed to run commands.         |
| `host`    | The hostname(s) this rule applies to (usually `ALL`).  |
| `(runas)` | Optional: user to run commands as (default is `root`). |
| `command` | The command(s) the user can execute.                   |

---

## 4. Examples

**Allow a user to run all commands as root:**

```
johndoe ALL=(ALL) ALL
```

**Allow a user to run specific commands:**

```
johndoe ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl
```

**Allow all members of a group to run all commands:**

```
%admin ALL=(ALL) ALL
```

**Run commands without a password prompt:**

```
johndoe ALL=(ALL) NOPASSWD: ALL
```

---

## 5. Useful `sudo` Options

| Option    | Description                                        |
| --------- | -------------------------------------------------- |
| `-l`      | List allowed commands for the current user.        |
| `-v`      | Validate your cached credentials (extend timeout). |
| `-k`      | Invalidate cached credentials.                     |
| `-u user` | Run command as another user.                       |
| `-i`      | Run login shell as root.                           |
| `-s`      | Run shell as root.                                 |

**Examples:**

```bash
# List allowed commands
sudo -l

# Run a command as another user
sudo -u postgres psql

# Open a root shell
sudo -i
```

---

## 6. Groups and Sudo

* Common group for sudo access: `sudo` (Debian/Ubuntu) or `wheel` (RedHat/CentOS).
* Add a user to sudo group:

```bash
sudo usermod -aG sudo johndoe   # Debian/Ubuntu
sudo usermod -aG wheel johndoe  # RedHat/CentOS
```

---

## 7. Security Tips

* Avoid giving `NOPASSWD` unless necessary.
* Limit the commands users can execute to the minimum required.
* Regularly audit `/etc/sudoers` and group memberships.
* Use `visudo` for safe editing; it checks for syntax errors.

---

## 8. `sudoers` File Advanced Features

**Include other files:**

```
# Include additional sudoers files
@includedir /etc/sudoers.d
```

**Defaults options:**

```bash
Defaults        timestamp_timeout=10   # Timeout in minutes for sudo password caching
Defaults        !requiretty           # Don't require tty for sudo
Defaults        logfile=/var/log/sudo.log
```

**Limiting commands to certain users or hosts:**

```bash
johndoe  webserver=(root) /usr/bin/systemctl restart apache2
```

---

## 9. Summary

* `sudo` provides controlled root access.
* `sudoers` file determines who can run what commands and as which user.
* Use `visudo` to safely edit `sudoers`.
* Always follow the **principle of least privilege**.

---

This guide serves as a reference for managing `sudo` access safely and effectively on Linux systems.



