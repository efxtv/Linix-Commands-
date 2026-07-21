# Linux Process Management Course by [EFXTv](https://t.me/efxtv)

## A Friendly, Step-by-Step Guide for Everyone

**Welcome to the Course!**

This is not a boring textbook. Think of this as your friendly teacher explaining how Linux manages programs — like explaining to a curious kid who wants to understand how their computer works.

We will go topic by topic, one by one. Each section is written in simple, human language with real examples.

---

## Table of Contents

1. [Introduction to Linux Jobs](#1-introduction-to-linux-jobs)
2. [Running Programs in Background (`&`)](#2-running-programs-in-background-)
3. [`jobs` Command](#3-jobs-command)
4. [`bg` Command](#4-bg-command)
5. [`fg` Command](#5-fg-command)
6. [Ctrl + Z — Pause a Program](#6-ctrl--z--pause-a-program)
7. [`kill` Command](#7-kill-command)
8. [Linux Signals (`kill -l`)](#8-linux-signals-kill--l)
9. [`pkill` Command](#9-pkill-command)
10. [`killall` Command](#10-killall-command)
11. [`pgrep` Command](#11-pgrep-command)
12. [`pidof` Command](#12-pidof-command)
13. [`ps` Command](#13-ps-command)
14. [`top` Command](#14-top-command)
15. [`htop` Command](#15-htop-command)
16. [`pstree` Command](#16-pstree-command)
17. [`nohup` Command](#17-nohup-command)
18. [`disown` Command](#18-disown-command)
19. [`wait` Command](#19-wait-command)
20. [`nice` and `renice`](#20-nice-and-renice)
21. [`taskset` Command](#21-taskset-command)
22. [`lsof` Command](#22-lsof-command)
23. [`fuser` Command](#23-fuser-command)
24. [`screen` and `tmux`](#24-screen-and-tmux)
25. [`cron`, `at`, and `batch`](#25-cron-at-and-batch)
26. [`strace` Command](#26-strace-command)
27. [`gdb` — The Debugger](#27-gdb--the-debugger)
28. [`perf` Tool](#28-perf-tool)
29. [The `/proc` Filesystem](#29-the-proc-filesystem)
30. [Best Practices for Safely Managing Processes](#30-best-practices-for-safely-managing-processes)

---

## 1. Introduction to Linux Jobs

Imagine your computer is a big playground. Every time you run a program (like a game, a music player, or a web browser), it becomes a **player** on that playground.

In Linux, we call these players **processes** or **jobs**.

A **job** is simply a program that is running. The Linux shell (the place where you type commands) keeps track of these jobs.

Why is this important?
- You can run many jobs at the same time
- You can pause them
- You can move them to the background
- You can stop them when you want

This is called **Job Control**.

---

## 2. Running Programs in Background (`&`)

Normally when you run a command, your terminal waits for it to finish. This is called **foreground**.

But what if you want to run something and still use the terminal?

Just add `&` at the end!

**Example:**
```bash
sleep 100 &
```

What happens?
- The `sleep 100` command starts
- It goes to the **background**
- You immediately get your prompt back

You can now type other commands while `sleep` is running in the background.

**Real life example:**
```bash
python3 download_big_file.py &
```

Now you can keep working while the file downloads.

---

## 3. `jobs` Command

The `jobs` command shows you all the programs you have started in your current terminal.

**How to use it:**
```bash
jobs
```

**Example output:**
```
[1]   Running                 sleep 100 &
[2]-  Stopped                 vim notes.txt
[3]+  Running                 python3 server.py
```

What do the symbols mean?
- `[1]`, `[2]`, `[3]` = Job numbers
- `Running` = The job is working
- `Stopped` = The job is paused
- `+` = Most recent job
- `-` = Second most recent job

This command is like checking your list of open apps.

---

## 4. `bg` Command

`bg` means **background**.

If you paused a program using `Ctrl + Z`, you can make it continue running in the background using `bg`.

**Example:**
```bash
# You are running a command, then press Ctrl+Z
# Now type:
bg
```

Or for a specific job:
```bash
bg %2
```

This is like saying: "Hey, keep working, but do it quietly in the background."

---

## 5. `fg` Command

`fg` means **foreground**.

It brings a background or stopped job back to the front so you can see it and interact with it.

**Examples:**
```bash
fg           # Bring the most recent job
fg %1        # Bring job number 1
fg %vim      # Bring the job that has "vim" in it
```

Think of it like switching tabs in your browser.

---

## 6. Ctrl + Z — Pause a Program

This is a magic keyboard shortcut!

When a program is running and you want to **pause** it (not kill it), just press:

**Ctrl + Z**

What happens?
- The program stops running
- It goes into "Stopped" state
- You get your terminal prompt back

You can later resume it with `bg` or `fg`.

**Example use case:**
You are editing a file with `vim`, but you suddenly want to check something. Press `Ctrl + Z`, do your work, then type `fg` to come back to vim.

---

## 7. `kill` Command

The `kill` command is used to **stop** a running process.

You need to give it the **Process ID (PID)**.

**Basic usage:**
```bash
kill 1234
```

This sends a polite "please stop" message.

If the program doesn't listen, you can be more forceful:
```bash
kill -9 1234
```

**How to find the PID?**
Use `jobs -l` or `ps` command.

---

## 8. Linux Signals (`kill -l`)

When you use `kill`, you are actually sending **signals** to the process.

Signals are like different types of messages.

To see all signals:
```bash
kill -l
```

Common signals:

| Signal     | Number | Meaning                          | What it does                     |
|------------|--------|----------------------------------|----------------------------------|
| SIGTERM    | 15     | Polite request to stop           | Default when you use `kill`      |
| SIGKILL    | 9      | Force kill (cannot be ignored)   | Most powerful                    |
| SIGINT     | 2      | Interrupt (like Ctrl+C)          | Stops the program                |
| SIGSTOP    | 19     | Pause the process                | Same as Ctrl+Z                   |
| SIGCONT    | 18     | Continue a paused process        | Used by `bg`                     |

You can send any signal like this:
```bash
kill -SIGKILL 1234
kill -9 1234
```

---

## 9. `pkill` Command

`pkill` is like `kill`, but you can kill processes by **name** instead of PID.

**Examples:**
```bash
pkill firefox
pkill -9 python
pkill -f "python server.py"
```

This is very useful when you don't remember the PID.

---

## 10. `killall` Command

`killall` kills **all** processes that have the same name.

**Example:**
```bash
killall chrome
killall -9 firefox
```

**Warning:** Be careful! If you have many `python` programs, it will kill all of them.

---

## 11. `pgrep` Command

`pgrep` is the opposite of `pkill`. It **finds** processes by name and shows their PIDs.

**Examples:**
```bash
pgrep firefox
pgrep -l python          # Show PID + name
pgrep -u yourname        # Processes of a user
```

Very handy for checking if something is running.

---

## 12. `pidof` Command

`pidof` finds the PID of a running program by its name.

**Example:**
```bash
pidof firefox
pidof python3
```

Simple and direct.

---

## 13. `ps` Command

`ps` stands for **Process Status**. It shows running processes.

**Most useful commands:**

```bash
ps aux                  # Show all processes
ps aux | grep python    # Find python processes
ps -ef                  # Full format
ps axjf                 # Show process tree
```

This is one of the most important commands in Linux.

---

## 14. `top` Command

`top` shows running processes in real-time, sorted by CPU usage.

**How to use:**
```bash
top
```

While `top` is running, you can press:
- `q` → Quit
- `k` → Kill a process
- `h` → Help

It's like a live dashboard of your computer.

---

## 15. `htop` Command

`htop` is a better, more colorful version of `top`.

It has:
- Mouse support
- Easier to read
- Colors
- Scrolling

If `htop` is not installed, you can install it:
```bash
sudo apt install htop
```

Many people prefer `htop` over `top`.

---

## 16. `pstree` Command

`pstree` shows processes in a **tree structure** — like a family tree.

**Example:**
```bash
pstree
pstree -p          # Show PIDs too
```

This helps you understand which process started which other process.

---

## 17. `nohup` Command

`nohup` means **"no hang up"**.

When you close your terminal, normally all background jobs die. `nohup` prevents that.

**Example:**
```bash
nohup python3 my_script.py &
```

Even if you close the terminal or log out, the script keeps running.

---

## 18. `disown` Command

`disown` removes a job from the shell's list.

After using `disown`, the job won't appear in `jobs` anymore, and it will keep running even if you close the terminal.

**Example:**
```bash
python3 server.py &
disown
```

---

## 19. `wait` Command

The `wait` command makes the shell **wait** for a background job to finish.

**Example:**
```bash
sleep 10 &
wait
echo "All done!"
```

This is mostly used in shell scripts.

---

## 20. `nice` and `renice`

These commands control how much CPU time a process gets.

- `nice` → Start a program with lower priority
- `renice` → Change priority of a running program

**Examples:**
```bash
nice -n 10 long_running_task &
renice -n 5 1234
```

Lower number = higher priority (more CPU time)

---

## 21. `taskset` Command

`taskset` lets you run a program on **specific CPU cores**.

**Example:**
```bash
taskset -c 0,1 python3 heavy_script.py
```

This is useful on computers with many CPU cores.

---

## 22. `lsof` Command

`lsof` = **List Open Files**

It shows which files and network connections are being used by processes.

**Useful commands:**
```bash
lsof -i :8080           # Who is using port 8080?
lsof /var/log/syslog
```

Very powerful for debugging.

---

## 23. `fuser` Command

`fuser` finds which process is using a file or port.

**Examples:**
```bash
fuser 8080/tcp
fuser /home/user/file.txt
```

---

## 24. `screen` and `tmux`

These are **terminal multiplexers**. They let you run multiple terminals inside one terminal.

They are very useful because:
- Your programs keep running even if you disconnect
- You can have many sessions

**Basic usage:**
```bash
screen
tmux
```

Once inside, press `Ctrl+A` then `D` to detach.

These are like having multiple tabs but in one window.

---

## 25. `cron`, `at`, and `batch`

These tools let you run commands **automatically** at specific times.

### `cron`
Runs commands on a schedule (every day, every hour, etc.)

Edit with:
```bash
crontab -e
```

### `at`
Run a command **once** at a future time.

```bash
at 5:00 PM
```

### `batch`
Like `at`, but runs when the system is not busy.

---

## 26. `strace` Command

`strace` shows what a program is doing behind the scenes (system calls).

**Example:**
```bash
strace ls
```

Great for debugging when a program is not working.

---

## 27. `gdb` — The Debugger

`gdb` is a powerful tool to debug programs (especially C and C++).

You can:
- Pause programs
- See variables
- Step through code line by line

**Basic usage:**
```bash
gdb ./my_program
```

This is mostly used by programmers.

---

## 28. `perf` Tool

`perf` is used to analyze **performance** of programs.

It tells you which parts of a program are using the most CPU.

**Example:**
```bash
perf top
perf record ./my_program
```

Used by advanced users and developers.

---

## 29. The `/proc` Filesystem

`/proc` is a special folder in Linux that contains information about running processes.

You can explore it like this:
```bash
ls /proc
cat /proc/1234/status
cat /proc/cpuinfo
```

It's like a magic folder that shows live system information.

---

## 30. Best Practices for Safely Managing Processes

Here are important rules to follow:

1. **Always check before killing**
   - Use `jobs`, `ps`, or `pgrep` first

2. **Prefer polite signals first**
   - Try `kill` (SIGTERM) before `kill -9`

3. **Use job numbers when possible**
   - `kill %2` is safer than guessing PIDs

4. **Redirect output in background**
   ```bash
   command > /dev/null 2>&1 &
   ```

5. **Use `nohup` or `disown`** for long-running tasks

6. **Don't kill important system processes**
   - Be careful with `killall`

7. **Use `htop`** — it's safer and easier than typing commands

8. **Test in a safe environment** when learning

---

## Final Words

Congratulations! You have now learned almost everything about managing processes in Linux.

Remember: Linux gives you **a lot of power**. Use it responsibly.

Practice these commands one by one. The more you use them, the more natural they will feel.

Happy learning!

---

*End of Course*
