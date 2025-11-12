Linux Basic Commands 1-20 Commands Class 1
`basic_linux_commands.md`.

---

````markdown
# 🐧 Basic Linux Commands — Explained Simply

This guide covers 20 essential Linux terminal commands that every beginner should know.  
They help you **navigate**, **view**, **create**, and **edit** files and folders in your system.

---

## 🧭 1. `pwd` — Print Working Directory
**Meaning:** Shows the directory (folder) you’re currently in.

**Example:**
```bash
pwd
````

**Output:**

```
/home/student/Documents
```

---

## 📂 2. `ls` — List Directory Contents

**Meaning:** Displays files and folders inside the current directory.

**Examples:**

```bash
ls
ls -l
ls -a
```

* `-l` → detailed info (permissions, size, date)
* `-a` → includes hidden files (those starting with `.`)

---

## 🚪 3. `cd` — Change Directory

**Meaning:** Moves you to another folder.

**Examples:**

```bash
cd Documents
cd ..
cd ~
cd /
```

* `..` → up one level
* `~` → home directory
* `/` → root directory

---

## 🏗️ 4. `mkdir` — Make Directory

**Meaning:** Creates a new folder.

**Examples:**

```bash
mkdir projects
mkdir -p myfolder/subfolder1/subfolder2
```

The `-p` flag creates nested folders.

---

## 🧹 5. `rmdir` — Remove Directory

**Meaning:** Deletes an *empty* directory.

**Example:**

```bash
rmdir old_folder
```

If the folder isn’t empty, use `rm -r`.

---

## ⚠️ 6. `rm` — Remove Files or Folders

**Meaning:** Deletes files or folders **permanently** (no trash bin).

**Examples:**

```bash
rm file.txt
rm -r folder_name
rm -rf folder_name
```

⚠️ `-rf` forces deletion — use with caution!

---

## 📄 7. `cp` — Copy Files or Folders

**Meaning:** Duplicates files or directories.

**Examples:**

```bash
cp notes.txt backup_notes.txt
cp -r folder1 folder2
```

The `-r` flag means “recursive,” for copying folders.

---

## 🚚 8. `mv` — Move or Rename Files

**Meaning:** Moves or renames files/folders.

**Examples:**

```bash
mv file.txt /home/student/Documents/
mv oldname.txt newname.txt
```

---

## 📖 9. `cat` — Show File Contents

**Meaning:** Displays the whole content of a file.

**Examples:**

```bash
cat notes.txt
cat file1.txt file2.txt > combined.txt
```

---

## 🔁 10. `tac` — Reverse of `cat`

**Meaning:** Shows file contents from bottom to top.

**Example:**

```bash
tac notes.txt
```

---

## 📜 11. `less` — View Files Page by Page (Interactive)

**Meaning:** Opens large files for easy scrolling.

**Example:**

```bash
less bigfile.txt
```

Use:

* `↑` / `↓` or Space → scroll
* `q` → quit

---

## 📄 12. `more` — View Files Page by Page (Simpler)

**Meaning:** Similar to `less`, but older and more basic.

**Example:**

```bash
more longtext.txt
```

Press **Space** for next page, **q** to quit.

---

## 🔝 13. `head` — Show Beginning of a File

**Meaning:** Displays the first 10 lines by default.

**Examples:**

```bash
head notes.txt
head -n 5 notes.txt
```

---

## 🔚 14. `tail` — Show End of a File

**Meaning:** Displays the last 10 lines by default.

**Examples:**

```bash
tail notes.txt
tail -n 20 notes.txt
tail -f log.txt
```

`-f` follows updates in real-time — useful for monitoring logs.

---

## 💬 15. `echo` — Print Text or Variables

**Meaning:** Prints text or variable values on screen.

**Examples:**

```bash
echo Hello World
echo $HOME
echo "My name is $USER"
```

---

## 📘 16. `touch` — Create Empty File or Update Timestamp

**Meaning:** Creates a new empty file or updates a file’s “last modified” time.

**Examples:**

```bash
touch newfile.txt
touch file1.txt file2.txt
```

---

## ✏️ 17. `nano` — Simple Text Editor

**Meaning:** Opens a beginner-friendly editor in the terminal.

**Example:**

```bash
nano notes.txt
```

**Shortcuts:**

* `Ctrl + O` → save
* `Ctrl + X` → exit
* `Ctrl + K` → cut line
* `Ctrl + U` → paste line

---

## 💻 18. `vi` — Classic Terminal Editor

**Meaning:** A powerful but older text editor.

**Example:**

```bash
vi file.txt
```

**Modes:**

* `i` → insert text
* `Esc` → exit insert mode
* `:wq` → save & quit
* `:q!` → quit without saving

---

## 🧠 19. `vim` — Vi Improved

**Meaning:** An enhanced version of `vi` with colors and more features.

**Example:**

```bash
vim script.sh
```

**Common Commands:**

* `i` → insert
* `Esc` → command mode
* `:w` → save
* `:q` → quit
* `:wq` → save & quit
* `u` → undo
* `/word` → search text

---

## 🧽 20. `clear` — Clear the Terminal Screen

**Meaning:** Clears everything currently visible in the terminal.

**Examples:**

```bash
clear
```

or use the shortcut:

```
Ctrl + L
```

This simply gives you a clean screen but doesn’t delete any data or history.

---

## 🧩 Summary Table

| #  | Command | Description            | Example                   |
| -- | ------- | ---------------------- | ------------------------- |
| 1  | `pwd`   | Show current directory | `pwd`                     |
| 2  | `ls`    | List files             | `ls -l`                   |
| 3  | `cd`    | Change directory       | `cd Documents`            |
| 4  | `mkdir` | Create folder          | `mkdir new_folder`        |
| 5  | `rmdir` | Delete empty folder    | `rmdir old`               |
| 6  | `rm`    | Delete files/folders   | `rm -r myfolder`          |
| 7  | `cp`    | Copy files             | `cp file1.txt backup.txt` |
| 8  | `mv`    | Move or rename         | `mv file.txt folder/`     |
| 9  | `cat`   | Show file              | `cat file.txt`            |
| 10 | `tac`   | Show file reversed     | `tac file.txt`            |
| 11 | `less`  | Scroll through file    | `less file.txt`           |
| 12 | `more`  | View file page by page | `more file.txt`           |
| 13 | `head`  | Show start of file     | `head file.txt`           |
| 14 | `tail`  | Show end of file       | `tail -n 20 file.txt`     |
| 15 | `echo`  | Print text             | `echo Hello`              |
| 16 | `touch` | Create empty file      | `touch new.txt`           |
| 17 | `nano`  | Simple editor          | `nano notes.txt`          |
| 18 | `vi`    | Classic editor         | `vi notes.txt`            |
| 19 | `vim`   | Enhanced editor        | `vim notes.txt`           |
| 20 | `clear` | Clear terminal screen  | `clear`                   |



### 🧠 Author Notes

Created for Fedora/Linux beginners learning to navigate the terminal.
Ideal for classroom teaching or self-learning.

**Maintained by:** [EFXTv]
**License:** MIT
**Last Updated:** November 2025


