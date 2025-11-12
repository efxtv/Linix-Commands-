# 🗂 Types of Files in Linux

When you run `ls -l`, the **first character** of each line indicates the file type:

```
-rw-r--r--  1 user user  1024 Nov 12 12:00 file.txt
drwxr-xr-x  2 user user  4096 Nov 12 12:00 mydir
lrwxrwxrwx  1 user user    11 Nov 12 12:00 link -> file.txt
```

Here, `-`, `d`, `l` are **file type symbols**.

---

### 1️⃣ Regular File (`-`)

* Symbol: `-` (dash)
* Description: Normal file containing **text, code, or data**.
* Example: `.txt`, `.jpg`, `.pdf`, `.sh`
* Permissions shown after symbol, e.g., `-rw-r--r--`

---

### 2️⃣ Directory (`d`)

* Symbol: `d`
* Description: Contains **other files and directories**.
* Example: `/home/user/Documents`
* Permissions apply to directory content: read (`r`), write (`w`), execute (`x`)

  * Execute (`x`) on a directory means you can **enter it with `cd`**.

---

### 3️⃣ Symbolic Link (`l`)

* Symbol: `l`
* Description: **Shortcut to another file or directory** (like Windows shortcuts).
* Example: `link -> /etc/passwd`
* Can point to files **inside or outside the current directory**.

---

### 4️⃣ Character Device (`c`)

* Symbol: `c`
* Description: Represents **hardware devices** that send/receive data **one character at a time**.
* Example: `/dev/tty0`, `/dev/zero`

---

### 5️⃣ Block Device (`b`)

* Symbol: `b`
* Description: Represents **hardware devices** that read/write **in blocks**.
* Example: `/dev/sda`, `/dev/nvme0n1`

---

### 6️⃣ FIFO / Named Pipe (`p`)

* Symbol: `p`
* Description: **Pipe** used to pass data **between processes**.
* Example: `mkfifo mypipe`
* Think of it as a **“data stream” between programs**.

---

### 7️⃣ Socket (`s`)

* Symbol: `s`
* Description: Special file for **network communication** or **inter-process communication** (IPC).
* Example: `/var/run/docker.sock`
* Programs use it to **send/receive data efficiently**.

---

### 8️⃣ Sticky Bit (`t`) and Setuid/Setgid (Special Permissions)

* Not a file type, but **special permission**
* Symbol: `t` appears on the **last execute bit** for directories
* Description: Prevents **users from deleting others’ files** in a directory.
* Example: `/tmp` usually has sticky bit: `drwxrwxrwt`
* **Setuid (`s`)** → Program runs as **owner** of the file
* **Setgid (`s`)** → Group ID is inherited when creating files in a directory

---

### ✅ Summary Table

| Symbol | Type              | Description                       |
| ------ | ----------------- | --------------------------------- |
| `-`    | Regular file      | Text, binary, scripts, etc.       |
| `d`    | Directory         | Contains files/folders            |
| `l`    | Symbolic link     | Shortcut to a file/folder         |
| `c`    | Character device  | Hardware (keyboard, terminal)     |
| `b`    | Block device      | Hardware (hard disk, SSD)         |
| `p`    | FIFO / Named Pipe | Inter-process communication       |
| `s`    | Socket            | Network/IPC communication         |
| `t`    | Sticky bit        | Restricts deletion in directories |
| `s`    | Setuid/Setgid     | Special execution permissions     |

---

### 🔹 How to Check File Types

You can use the `ls -l` command:

```bash
ls -l
```

Or use `file` command for a **more descriptive type**:

```bash
file /dev/tty0
file /home/user/file.txt
```

---

If you want, I can make a **visual diagram showing all Linux file types** like a flowchart with **examples** for your GitHub post.
It will make it **super easy for students to memorize**.

Do you want me to do that next?
