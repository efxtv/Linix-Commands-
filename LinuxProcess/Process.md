# All Types of Processes in Linux — A Comprehensive Course

---

## Table of Contents

1. [What Is a Process?](#1-what-is-a-process)
2. [Process Anatomy](#2-process-anatomy)
3. [Classification by Origin](#3-classification-by-origin)
4. [Classification by Execution Mode](#4-classification-by-execution-mode)
5. [Classification by Scheduling](#5-classification-by-scheduling)
6. [Classification by Parent-Child Relationship](#6-classification-by-parent-child-relationship)
7. [Classification by Lifecycle State](#7-classification-by-lifecycle-state)
8. [Classification by Privilege Level](#8-classification-by-privilege-level)
9. [Classification by Session and Terminal Association](#9-classification-by-session-and-terminal-association)
10. [Special Process Types](#10-special-process-types)
11. [Process Creation Mechanisms](#11-process-creation-mechanisms)
12. [Process Management Commands](#12-process-management-commands)
13. [Practical Exercises](#13-practical-exercises)
14. [Summary Reference Table](#14-summary-reference-table)

---

## 1. What Is a Process?

A **process** is a program in execution. It is the fundamental unit of work in Linux. When a binary executable is loaded into memory and the CPU begins executing its instructions, it becomes a process.

Every process in Linux is assigned a unique numeric identifier called the **Process ID (PID)**. The kernel tracks all processes through the `task_struct` data structure, which resides in kernel memory and contains all metadata about the process.

Key facts:
- PIDs are of type `pid_t` and are unique at any given moment.
- The maximum PID value is defined in `/proc/sys/kernel/pid_max` (default 32768, can be raised to 4194304).
- PID 0 is the idle/swapper process (not visible in process listings).
- PID 1 is the `init` process (or systemd), the ancestor of all user-space processes.

---

## 2. Process Anatomy

Each process is represented internally by a **`task_struct`** (defined in `include/linux/sched.h`). Key fields include:

| Field | Description |
|---|---|
| `pid` | Process ID |
| `tgid` | Thread Group ID (PID of the main thread) |
| `state` | Current execution state (running, sleeping, etc.) |
| `mm` | Memory descriptor (points to `mm_struct`) |
| `files` | Open file descriptors table |
| `fs` | Filesystem information (root dir, cwd, umask) |
| `signal` | Signal handlers and pending signals |
| `parent` | Pointer to parent `task_struct` |
| `children` | List of child processes |
| `cred` | Credentials (UID, GID, capabilities) |
| `comm` | Command name (truncated to 15 chars + null) |

---

## 3. Classification by Origin

### 3.1 Kernel Processes (Kernel Threads)

Kernel processes run entirely in **kernel space** and have no user-space memory context. They are created using `kthread_create()` and perform internal kernel tasks.

**Characteristics:**
- No user memory space (`mm` pointer is NULL).
- Listed with square brackets in `ps` and `top` (e.g., `[kthreadd]`).
- Parent is always PID 2 (`kthreadd`).
- Cannot be killed with `SIGKILL` in most cases.
- Run with highest priority.

**Common kernel threads:**

| Process | Purpose |
|---|---|
| `[kthreadd]` (PID 2) | Parent of all kernel threads; spawns them on demand |
| `[ksoftirqd/N]` | Handles soft interrupts for CPU N |
| `[kworker/N:M]` | Workqueue threads for deferred kernel work on CPU N, instance M |
| `[rcu_sched]`, `[rcu_bh]`, `[rcu_preempt]` | RCU (Read-Copy-Update) grace period handling |
| `[migration/N]` | Handles process migration between CPUs |
| `[watchdog/N]` | Detects hard lockups on CPU N |
| `[kswapd0]` | Memory reclaim and page swapping daemon |
| `[jbd2/sda1-8]` | Journaling block device driver for ext4 on `/dev/sda1` |
| `[irq/N-name]` | Threaded IRQ handlers for interrupt N |
| `[scsi_tmf_N]` | SCSI task management function threads |
| `[khungtaskd]` | Detects tasks stuck in uninterruptible sleep |
| `[oom_reaper]` | Out-of-memory memory reaper |
| `[writeback]` | Flushes dirty pages to disk |
| `[crypto]` | Asynchronous crypto API workqueue |
| `[kblockd]` | Block layer workqueue |

**How to identify:**
```bash
ps -eo pid,comm | grep '\['
# or
ps -eo pid,comm --no-headers | awk '$2 ~ /^\[/'
```

---

### 3.2 User Processes

User processes execute in **user space** and have a full user memory context. They are what users typically interact with.

**Characteristics:**
- Have a valid `mm` (memory descriptor).
- Run with user-level privileges (unless setuid/setgid).
- Created via `fork()`/`exec()` family of system calls.
- Can receive and handle signals.
- Subject to resource limits (`ulimit`).

**Examples:** `bash`, `firefox`, `sshd`, `nginx`, `python3`, `vim`.

---

## 4. Classification by Execution Mode

### 4.1 Foreground Processes

A foreground process is attached to the terminal and **receives input from it**. Only one process (or process group) can be in the foreground at a time per terminal.

**Characteristics:**
- Receives keyboard input (stdin).
- Terminal is blocked until the process finishes.
- Receives signals from terminal (e.g., `SIGINT` from Ctrl+C).
- Has a terminal (`TTY`) associated with it.

**Example:**
```bash
$ vim file.txt
# Terminal is blocked until vim exits
```

---

### 4.2 Background Processes

A background process runs **without terminal input**. It is launched by appending `&` to the command or by sending a foreground process to background with `Ctrl+Z` followed by `bg`.

**Characteristics:**
- Does not block the terminal.
- Cannot read from terminal stdin (gets EOF or SIGTTIN if it tries).
- Still shares the terminal's stdout/stderr unless redirected.
- May receive `SIGHUP` when the terminal closes (unless disowned or nohup'd).

**Examples:**
```bash
$ find / -name "*.log" > /tmp/find.log &
[1] 12345

$ cp largefile.iso /backup/ &
[2] 12346
```

---

### 4.3 Daemons

Daemons are **long-running background processes** that provide services. They typically start at boot and run until shutdown.

**Characteristics:**
- No controlling terminal (TTY is `?`).
- Parent is usually PID 1 (or a service manager like systemd).
- Run in their own process group and session.
- Conventionally named with a trailing `d` (e.g., `sshd`, `httpd`, `crond`).
- Detach from terminal using `setsid()` and chdir to `/`.
- Redirect stdin, stdout, stderr to `/dev/null` or log files.
- Typically run as root initially, then drop privileges.

**Proper daemonization steps (classic Unix):**
1. Call `fork()` — parent exits, child continues.
2. Call `setsid()` — create new session, detach from controlling terminal.
3. Call `fork()` again (double-fork) — prevent acquiring a new controlling terminal.
4. Call `chdir("/")` — change working directory to root.
5. Call `umask(0)` — reset file mode creation mask.
6. Close all inherited file descriptors (0, 1, 2).
7. Redirect stdin/stdout/stderr to `/dev/null` or log files.

**Common daemons:**

| Daemon | Purpose |
|---|---|
| `systemd` (PID 1) | System and service manager |
| `sshd` | OpenSSH server |
| `cron` / `crond` | Scheduled task execution |
| `rsyslogd` | System logging |
| `cupsd` | Common Unix Printing System |
| `dbus-daemon` | D-Bus message bus system |
| `NetworkManager` | Network connection management |
| `ntpd` / `chronyd` | Network Time Protocol synchronization |
| `atd` | Deferred command execution |

**Modern note:** With `systemd`, traditional daemonization is often handled by the service manager itself. Many modern services are written as "simple" services that don't double-fork; systemd manages the daemonization.

**How to identify:**
```bash
ps -eo pid,tty,comm --no-headers | awk '$2 == "?"'
```

---

## 5. Classification by Scheduling

Linux uses the **Completely Fair Scheduler (CFS)** for normal processes and a real-time scheduler for RT processes. Processes are classified into scheduling policies:

### 5.1 Normal Processes (SCHED_OTHER / SCHED_NORMAL)

- Default scheduling policy.
- Managed by CFS (Completely Fair Scheduler).
- Priority is determined by the **nice value** (-20 to +19, where lower = higher priority).
- Default nice value is 0.
- Time slice is dynamic, based on nice value and system load.

```bash
# Check scheduling policy and nice value
ps -eo pid,comm,ni,policy --no-headers

# Start with specific nice value
nice -n 10 ./long_task.sh

# Change nice of running process
renice -n 5 -p 1234
```

### 5.2 Real-Time Processes (SCHED_FIFO, SCHED_RR)

Real-time processes have **higher priority** than normal processes and are never preempted by normal processes.

| Policy | Behavior |
|---|---|
| `SCHED_FIFO` | First-In-First-Out. Runs until it blocks, exits, or is preempted by a higher-priority RT process. **No time slicing.** |
| `SCHED_RR` | Round-Robin. Same as FIFO but with time slicing. When time quantum expires, process is moved to end of queue for its priority level. |

**RT Priority Range:** 1 (lowest RT) to 99 (highest RT). RT priorities always outrank nice values.

```bash
# Start a real-time process (requires CAP_SYS_NICE or root)
chrt -f 50 ./realtime_task    # SCHED_FIFO at priority 50
chrt -r 30 ./realtime_task    # SCHED_RR at priority 30

# Change scheduling of running process
chrt -f -p 80 1234             # Set PID 1234 to SCHED_FIFO priority 80

# Check scheduling
chrt -p 1234
```

### 5.3 Idle Process (SCHED_IDLE)

- Runs only when no other process wants the CPU.
- Used for very low-priority background tasks.
- Not the same as PID 0 (the kernel idle task).

```bash
chrt -i 0 ./batch_job.sh
```

### 5.4 Batch Process (SCHED_BATCH)

- Optimized for CPU-intensive, non-interactive batch jobs.
- Similar to SCHED_NORMAL but tuned for throughput rather than latency.
- Does not get interactive priority boosts.

```bash
chrt -b 0 ./compute_heavy_task
```

### 5.5 Deadline Scheduling (SCHED_DEADLINE)

- Introduced in Linux 3.14.
- Uses Earliest Deadline First (EDF) scheduling.
- Processes specify runtime, deadline, and period parameters.
- Used for hard real-time applications.

```bash
# Set deadline scheduling (requires root)
chrt -d -T 1000000 -P 500000 -D 800000 ./deadline_task
# -T period, -P runtime, -D deadline (in microseconds)
```

### 5.6 PID 0 — The Idle/Swapper Process

- Created at boot before any other process exists.
- Runs when no other process is runnable on a CPU.
- Executes the CPU's idle loop (often invoking `hlt` instruction to save power).
- One instance per CPU core.
- Not visible in `ps` output (not a normal task).
- Has no `mm` and runs exclusively in kernel space.
- Cannot be killed or modified.

---

## 6. Classification by Parent-Child Relationship

### 6.1 Parent Processes

A parent process is one that **creates child processes** via `fork()`. The parent can:
- Wait for children to terminate using `wait()` or `waitpid()`.
- Continue executing concurrently with children.
- Become the parent of new children after `fork()`.

```bash
# View parent PID (PPID)
ps -eo pid,ppid,comm --no-headers
```

### 6.2 Child Processes

A child process is created by a parent via `fork()`. After `fork()`:
- Child gets a new unique PID.
- Child inherits copies of parent's memory space (Copy-on-Write).
- Child inherits file descriptors, environment variables, working directory.
- `fork()` returns 0 in the child, child's PID in the parent.

```c
pid_t pid = fork();
if (pid == 0) {
    // Child process
    execvp("./child_program", args);
} else if (pid > 0) {
    // Parent process
    wait(NULL);  // Wait for child
}
```

### 6.3 Orphan Processes

An **orphan process** is a child whose parent has terminated or exited before the child.

**What happens:**
- Orphaned processes are **reparented** to `kthreadd` (PID 2) or PID 1 (systemd).
- The new parent handles the `wait()` to collect the exit status.
- The process continues running normally.

**Creating an orphan (demonstration):**
```bash
# Parent exits immediately; child becomes orphan
bash -c 'sleep 30 & exit'
# The sleep process is now orphaned and reparented to PID 1
```

### 6.4 Zombie Processes (Defunct Processes)

A **zombie process** is a process that has **terminated** but its **exit status has not yet been collected** by its parent via `wait()`.

**Characteristics:**
- Appears in `ps` with state `Z` and label `<defunct>`.
- Consumes no resources except a `task_struct` entry (minimal memory).
- Cannot be killed with `SIGKILL` (already dead).
- Entry remains in process table until parent calls `wait()`.
- If parent never calls `wait()`, zombie persists.

**How to identify:**
```bash
ps aux | awk '$8 ~ /^Z/'
# or
ps -eo pid,ppid,stat,comm | grep -w Z
```

**How to resolve:**
1. **Send SIGCHLD to parent** to prompt it to reap:
   ```bash
   kill -SIGCHLD <parent_pid>
   ```
2. **Kill the parent** — zombies are reparented to PID 1, which reaps them.
   ```bash
   kill <parent_pid>
   ```
3. **Fix the parent's code** — ensure it calls `wait()` or sets `SA_NOCLDWAIT`.

**Note:** A few zombies are harmless. Thousands indicate a buggy parent.

### 6.5 Init Process (PID 1)

The **init process** is the first user-space process started by the kernel.

**Characteristics:**
- Always has PID 1.
- Parent of all orphaned processes.
- Reaps all orphaned zombies (acts as final `wait()` collector).
- Cannot be killed by normal signals (SIGKILL is ignored).
- Responsible for system shutdown/halt when signaled.

**Evolution of init:**
| Init System | Era | Notes |
|---|---|---|
| SysVinit | 1980s–2000s | Serial startup, runlevels |
| Upstart | 2006–2014 | Event-driven (Ubuntu) |
| systemd | 2010–present | Parallel startup, sockets, D-Bus, cgroups (most distros) |
| OpenRC | 2007–present | Dependency-based (Alpine, Gentoo, Devuan) |
| runit | 2000s–present | Simple, fast (Void Linux) |
| s6 | 2010s–present | Supervision tree (Alpine option) |

### 6.6 Kthreadd Process (PID 2)

- Second process created by the kernel.
- Parent of all kernel threads.
- Spawns kernel threads on demand via `kthread_create()`.
- Runs the `kthreadd()` function in an infinite loop.

---

## 7. Classification by Lifecycle State

Every process has a **state** that reflects its current condition. States are visible via `ps` (STAT column) or `/proc/<pid>/status`.

### 7.1 Process States

| State Code | Name | Description |
|---|---|---|
| **R** | Running / Runnable | Currently executing on CPU, or in runqueue waiting to be scheduled |
| **S** | Interruptible Sleep | Waiting for an event (I/O, signal, timer). Woken by signal or event completion. |
| **D** | Uninterruptible Sleep | Waiting for I/O. **Cannot be interrupted by signals** (even SIGKILL). Usually indicates disk/NFS I/O. |
| **Z** | Zombie | Terminated but not yet reaped by parent. |
| **T** | Stopped | Halted by SIGSTOP, SIGTSTP, SIGTTIN, or SIGTTOU. Also appears during ptrace debugging. |
| **t** | Tracing Stop | Stopped by debugger (ptrace) during tracing. |
| **X** / **x** | Dead | Should never be seen in output; process is being removed from task list. |
| **I** | Idle (kernel thread) | Kernel thread is idle (Linux 4.14+). Not counted in load average. |
| **P** | Parked | CPU is hot-unplugged; process is parked (Linux 3.10+). |

### 7.2 State Modifiers

These characters can appear alongside the primary state in `ps` output:

| Modifier | Meaning |
|---|---|
| `<` | High-priority process (nice value < 0) |
| **N** | Low-priority process (nice value > 0) |
| **s** | Session leader |
| **l** | Multi-threaded (using CLONE_THREAD, e.g., pthreads) |
| **L** | Has pages locked in memory (real-time or custom) |
| **+** | In the foreground process group |

**Example interpretation:**
```
STAT = Ss+   → Interruptible sleep, session leader, foreground
STAT = SNl   → Interruptible sleep, low-priority, multi-threaded
STAT = R<    → Running, high-priority
STAT = Z     → Zombie
STAT = D     → Uninterruptible sleep (I/O wait)
```

### 7.3 State Transitions

```
                    fork()
  Parent ───────────────────────────────────> Child
                        |
                        v
                     (R) Running
                   /    |    \
               sched  wait()  exit()
                /      |       \
               v       v        v
              (R)    (S)/(D)   (Z) Zombie
             running  sleeping   |
                       |      wait()
                  signal/I/O     |
                       |         v
                       v       (Dead/X)
                     (R)
                    ready
```

---

## 8. Classification by Privilege Level

### 8.1 Root Processes

Processes running as **UID 0 (root)**.

**Characteristics:**
- Full access to system resources.
- Can bind to privileged ports (< 1024).
- Can change file ownership, load kernel modules.
- Can send signals to any process.
- Can override file permissions.

**Examples:** `systemd`, `sshd`, `kthreadd`, all kernel threads.

### 8.2 User Processes

Processes running under a **non-root UID**.

**Characteristics:**
- Restricted to user's own files and resources.
- Cannot bind to ports < 1024.
- Subject to `ulimit` restrictions.
- Can only send signals to processes with same UID (unless capabilities grant otherwise).

### 8.3 Setuid/Setgid Processes

Processes that **run with elevated privileges** different from the invoking user.

**Mechanism:**
- **Setuid bit** on executable: process runs with file owner's UID.
- **Setgid bit** on executable: process runs with file group's GID.

```bash
# Find setuid files
find / -perm -4000 -type f 2>/dev/null

# Find setgid files
find / -perm -2000 -type f 2>/dev/null

# Example: passwd is setuid root
ls -l /usr/bin/passwd
# -rwsr-xr-x 1 root root 68240 ... /usr/bin/passwd
#        ^
#    's' = setuid bit
```

**Common setuid binaries:**
- `/usr/bin/passwd` — allows users to change their password (needs root to write `/etc/shadow`).
- `/usr/bin/sudo` — executes commands as another user.
- `/usr/bin/su` — switch user.
- `/usr/bin/ping` — needs raw socket access (though modern Linux uses capabilities instead).
- `/usr/bin/newgrp` — change group ID.

**Security note:** Setuid binaries are a common attack vector. Minimize their number.

### 8.4 Capability-Based Processes

Linux **capabilities** split root privileges into distinct units. A process can have specific capabilities without being fully root.

**How to check:**
```bash
# View capabilities of a running process
cat /proc/<pid>/status | grep -i cap

# View capabilities of a file
getcap /usr/bin/ping
# /usr/bin/ping cap_net_raw=ep
```

**Common capabilities:**

| Capability | Permission |
|---|---|
| `CAP_NET_RAW` | Use raw and packet sockets |
| `CAP_NET_BIND_SERVICE` | Bind to ports < 1024 |
| `CAP_SYS_ADMIN` | Broad admin operations (mount, etc.) |
| `CAP_SYS_PTRACE` | Trace/debug other processes |
| `CAP_DAC_OVERRIDE` | Bypass file read/write/execute permission checks |
| `CAP_KILL` | Send signals to any process |
| `CAP_SYS_NICE` | Modify scheduling priority |
| `CAP_CHOWN` | Change file ownership |
| `CAP_FOWNER` | Bypass permission checks on operations requiring file owner UID match |
| `CAP_SETUID` / `CAP_SETGID` | Change UID/GID |

---

## 9. Classification by Session and Terminal Association

### 9.1 Session Leaders

A **session leader** is the first process in a session, created by `setsid()`.

**Characteristics:**
- SID (Session ID) equals its PID.
- Can have a controlling terminal.
- Typically a shell or daemon.

```bash
ps -eo pid,sid,tty,comm --no-headers | awk '$1 == $2'
```

### 9.2 Process Group Leaders

A **process group** is a collection of processes used for signal distribution (e.g., sending SIGINT to all processes in a pipeline).

**Characteristics:**
- PGID (Process Group ID) equals the leader's PID.
- All processes in a pipeline share the same PGID.
- Terminal sends signals to the foreground process group.

```bash
# All these share the same PGID
$ cmd1 | cmd2 | cmd3

ps -eo pid,pgid,comm
```

### 9.3 Terminal-Bound Processes

Processes associated with a **controlling terminal (TTY)**.

**Characteristics:**
- Receive terminal-generated signals (SIGHUP, SIGINT, SIGQUIT).
- Can read from stdin.
- TTY column in `ps` shows the terminal (e.g., `pts/0`, `tty1`).

### 9.4 Detached Processes (No Terminal)

Processes with **no controlling terminal** (TTY = `?`).

**Characteristics:**
- Do not receive terminal signals.
- Includes daemons, cron jobs, and processes started with `nohup`.
- Survive terminal closure.

---

## 10. Special Process Types

### 10.1 Threaded Processes

Linux threads are implemented as processes that share resources via the `clone()` system call with specific flags.

**Characteristics:**
- All threads in a process share the same PID (TGID = Thread Group ID).
- Each thread has a unique TID (Thread ID, which is its actual PID).
- Share memory space, file descriptors, signal handlers.
- In `ps`, shown with `l` state modifier.
- `ps -T` or `ps -m` shows individual threads.

```bash
# Show threads
ps -T -p <pid>
# or
ps -eLf | grep <process_name>
# or via /proc
ls /proc/<pid>/task/
```

**Thread creation flags (`clone()`):**
- `CLONE_VM` — share memory space.
- `CLONE_FS` — share filesystem info.
- `CLONE_FILES` — share file descriptors.
- `CLONE_SIGHAND` — share signal handlers.
- `CLONE_THREAD` — place in same thread group.

### 10.2 Container Processes

Processes running inside **containers** (Docker, LXC, Podman, etc.).

**Characteristics:**
- Have their own PID namespace — PID 1 inside container is different from host PID 1.
- Isolated via cgroups (resources) and namespaces (PID, network, mount, IPC, UTS, user).
- From the host perspective, they are normal processes with the container runtime as ancestor.
- `ps` inside container sees only container processes.

```bash
# View processes in a Docker container
docker top <container_name>

# View from host (shows real PIDs)
ps -ef | grep containerd

# Check namespace
ls -l /proc/<pid>/ns/
# pid: [4026532456]  ← different from host PID namespace
```

### 10.3 Initramfs Processes

Processes that run during the **initial RAM filesystem** stage, before the real root filesystem is mounted.

**Characteristics:**
- Run from initramfs (early boot).
- Responsible for loading drivers, mounting real root filesystem.
- PID 1 in initramfs hands over to the real init.

### 10.4 One-Shot Processes

Processes designed to **run once and exit**.

**Examples:**
- Scripts executed by `cron` or `at`.
- Systemd services with `Type=oneshot`.
- Build scripts, backup jobs.

```bash
# systemd oneshot service
systemctl list-units --type=service | grep oneshot
```

### 10.5 Supervised Processes

Processes managed by a **supervisor** that monitors and restarts them on failure.

**Supervisors:**
- `systemd` (most common).
- `supervisord`.
- `runit` / `sv`.
- `s6`.
- `monit`.

```bash
# Check if process is managed by systemd
systemctl status <service>

# List all supervised services
systemctl list-units --type=service --state=running
```

### 10.6 CGroup-Constrained Processes

Processes whose resource usage is **limited by cgroups** (control groups).

**Resource constraints:**
- CPU time (`cpu`, `cpuacct` controllers).
- Memory (`memory` controller).
- I/O bandwidth (`blkio` / `io` controller).
- PIDs (`pids` controller).
- Devices (`devices` controller).

```bash
# View cgroup membership of a process
cat /proc/<pid>/cgroup

# View all cgroups (systemd)
systemd-cgls

# View memory usage
cat /sys/fs/cgroup/memory/.../memory.usage_in_bytes
```

### 10.7 Namespaced Processes

Processes isolated in various **Linux namespaces**.

| Namespace | Isolates | Flag |
|---|---|---|
| PID | Process IDs | `CLONE_NEWPID` |
| Mount | Filesystem mount points | `CLONE_NEWNS` |
| Network | Network stack, interfaces, ports | `CLONE_NEWNET` |
| IPC | Inter-process communication | `CLONE_NEWIPC` |
| UTS | Hostname and domain name | `CLONE_NEWUTS` |
| User | User/group IDs | `CLONE_NEWUSER` |
| Cgroup | Cgroup root directory | `CLONE_NEWCGROUP` |
| Time | Clocks (Linux 5.6+) | `CLONE_NEWTIME` |

```bash
# Check namespaces
ls -l /proc/<pid>/ns/

# Create a process in new namespaces
unshare --pid --fork --mount-proc bash
```

---

## 11. Process Creation Mechanisms

### 11.1 `fork()`

Creates an **exact copy** of the calling process.

```c
pid_t pid = fork();
// Returns 0 in child, child's PID in parent, -1 on error
```

- Child inherits: memory (CoW), file descriptors, environment, cwd, umask.
- Child gets: new PID, new PPID, own resource usage counters (zeroed).

### 11.2 `vfork()`

Variant of `fork()` where **child shares the parent's memory** until it calls `exec()` or `exit()`.

- Parent is **suspended** until child calls `exec()` or `exit()`.
- Faster than `fork()` (no page table copy), but restricted usage.
- **Deprecated** in favor of `posix_spawn()`.

### 11.3 `clone()`

Low-level system call that `fork()`, `vfork()`, and `pthread_create()` are built on.

```c
int clone(int (*fn)(void *), void *child_stack, int flags, void *arg);
```

- Fine-grained control over what resources are shared.
- Used for implementing threads (`CLONE_THREAD`, `CLONE_VM`, etc.).
- Used for containers (`CLONE_NEWPID`, `CLONE_NEWNS`, etc.).

### 11.4 `exec()` Family

**Replaces** the current process image with a new program. Does not create a new process.

| Function | Description |
|---|---|
| `execl()` | Arguments as list, requires full path |
| `execle()` | `execl` + environment |
| `execlp()` | `execl` + PATH search |
| `execv()` | Arguments as array |
| `execve()` | `execv` + environment (the actual syscall) |
| `execvp()` | `execv` + PATH search |
| `execvpe()` | `execv` + PATH + environment |

**Typical pattern:**
```c
pid_t pid = fork();
if (pid == 0) {
    // Child: replace with new program
    execvp("ls", (char *[]){"ls", "-l", NULL});
    // If exec fails:
    _exit(127);
}
// Parent continues
waitpid(pid, &status, 0);
```

### 11.5 `posix_spawn()`

Higher-level interface combining fork and exec in one call.

```c
pid_t pid;
char *argv[] = {"ls", "-l", NULL};
posix_spawn(&pid, "/bin/ls", NULL, NULL, argv, environ);
```

- More efficient than fork+exec on some architectures.
- Safer for use in multi-threaded programs (avoids signal handler issues).

### 11.6 `system()`

Executes a command through the shell (`/bin/sh -c`).

```c
system("ls -l | grep .txt");
```

- **Warning:** Vulnerable to shell injection if input is untrusted.
- Blocks until command completes.
- Returns shell exit status.

---

## 12. Process Management Commands

### 12.1 Viewing Processes

| Command | Description |
|---|---|
| `ps aux` | BSD-style, all processes |
| `ps -ef` | System V-style, all processes |
| `ps -eo pid,ppid,stat,comm` | Custom format |
| `top` | Interactive process viewer |
| `htop` | Enhanced interactive viewer (needs install) |
| `btop` | Modern resource monitor (needs install) |
| `pgrep <name>` | Find PIDs by name |
| `pstree` | Display process tree |
| `pidof <name>` | Get PID(s) of running program |

### 12.2 Controlling Processes

| Command | Description |
|---|---|
| `kill <pid>` | Send SIGTERM (15) |
| `kill -9 <pid>` | Send SIGKILL (force kill) |
| `kill -SIGSTOP <pid>` | Stop process |
| `kill -SIGCONT <pid>` | Resume stopped process |
| `killall <name>` | Kill all processes by name |
| `pkill <pattern>` | Kill processes matching pattern |
| `kill -0 <pid>` | Check if process exists (no signal sent) |

### 12.3 Priority and Scheduling

| Command | Description |
|---|---|
| `nice -n <val> <cmd>` | Start with specific nice value |
| `renice -n <val> -p <pid>` | Change nice of running process |
| `chrt -f <pri> <cmd>` | Start with SCHED_FIFO |
| `chrt -r <pri> <cmd>` | Start with SCHED_RR |
| `chrt -p <pid>` | Check scheduling policy |
| `ionice -c <class> -n <val> <cmd>` | Set I/O scheduling class/priority |

### 12.4 Process Inspection

| Path / Command | Description |
|---|---|
| `/proc/<pid>/` | Process information directory |
| `/proc/<pid>/status` | Human-readable status |
| `/proc/<pid>/cmdline` | Command line (null-separated) |
| `/proc/<pid>/fd/` | Open file descriptors |
| `/proc/<pid>/maps` | Memory mappings |
| `/proc/<pid>/environ` | Environment variables |
| `/proc/<pid>/stack` | Kernel stack trace |
| `/proc/<pid>/stat` | Machine-readable process stats |
| `strace -p <pid>` | Trace system calls |
| `lsof -p <pid>` | List open files |

---

## 13. Practical Exercises

### Exercise 1: Identify Process Types

```bash
# List all kernel threads
ps -eo pid,comm | grep '\['

# List all daemons (no TTY)
ps -eo pid,tty,comm --no-headers | awk '$2 == "?"'

# List foreground processes
ps -eo pid,stat,tty,comm --no-headers | grep '+'

# Find zombie processes
ps -eo pid,ppid,stat,comm | awk '$3 ~ /^Z/'
```

### Exercise 2: Process State Transitions

```bash
# Create a sleeping process
sleep 300 &
ps -p $! -o pid,stat,comm   # State: S

# Stop it
kill -STOP $!
ps -p $! -o pid,stat,comm   # State: T

# Resume it
kill -CONT $!
ps -p $! -o pid,stat,comm   # State: S

# Create a zombie
bash -c 'exit' &
# Parent is bash; zombie will be quickly reaped
# To observe, use a custom C program that forks but doesn't wait
```

### Exercise 3: Scheduling Policies

```bash
# Check default policy
chrt -p $$

# View all scheduling policies
chrt --help  # Shows: OTHER, FIFO, RR, BATCH, ISO, IDLE, DEADLINE

# Start low-priority batch job
chrt -i 0 nice -n 19 find / > /dev/null 2>&1 &

# Check its scheduling
chrt -p $(pgrep -f "find /")
```

### Exercise 4: Process Tree Analysis

```bash
# Display full process tree
pstree -p

# Show process tree with user info
pstree -p -u

# Show specific process and its children
pstree -p <pid>

# View ancestors of current shell
cat /proc/$$/status | grep PPid
```

### Exercise 5: Namespace and Container Inspection

```bash
# Check namespaces of a process
ls -l /proc/self/ns/

# Create isolated PID namespace
unshare --pid --fork --mount-proc ps aux

# Compare with host
ps aux | wc -l    # Many processes
unshare --pid --fork --mount-proc ps aux | wc -l  # Few processes (PID 1 = ps)
```

---

## 14. Summary Reference Table

| Classification | Types | Key Identifiers |
|---|---|---|
| **By Origin** | Kernel threads, User processes | `[brackets]` in ps, `mm` field |
| **By Execution** | Foreground, Background, Daemons | TTY field, `&`, `+` modifier |
| **By Scheduling** | Normal, RT (FIFO/RR), Idle, Batch, Deadline | `chrt -p`, `SCHED_*` policies |
| **By Relationship** | Parent, Child, Orphan, Zombie, Init (PID 1), Kthreadd (PID 2) | PPID, STAT `Z`, reparenting |
| **By State** | R, S, D, Z, T, t, I, X | STAT column in `ps` |
| **By Privilege** | Root, User, Setuid/Setgid, Capability-based | UID, `s` bit, `getcap` |
| **By Session** | Session leader, Process group, TTY-bound, Detached | SID, PGID, TTY field |
| **Special** | Threads, Containers, Supervised, Cgroup-constrained, Namespaced | `/proc/<pid>/task/`, namespaces |

---

## Quick Signal Reference

| Signal | Number | Default Action | Typical Use |
|---|---|---|---|
| `SIGHUP` | 1 | Terminate | Terminal hangup / reload config |
| `SIGINT` | 2 | Terminate | Ctrl+C |
| `SIGQUIT` | 3 | Core dump | Ctrl+\ |
| `SIGKILL` | 9 | Terminate (cannot catch) | Force kill |
| `SIGTERM` | 15 | Terminate | Graceful shutdown (default for `kill`) |
| `SIGSTOP` | 19 | Stop (cannot catch) | Pause process |
| `SIGCONT` | 18 | Continue | Resume stopped process |
| `SIGCHLD` | 17 | Ignore | Child stopped/terminated |

---

## Further Reading

- `man 2 fork`, `man 2 execve`, `man 2 clone` — Process creation syscalls
- `man 7 signal` — Signal handling
- `man 5 proc` — `/proc` filesystem
- `man 1 ps` — Process status
- `man 1 chrt` — Real-time scheduling
- `man 1 sched` — CPU affinity
- Linux kernel source: `kernel/fork.c`, `kernel/sched/`, `include/linux/sched.h`
- "The Linux Programming Interface" by Michael Kerrisk — comprehensive reference
- "Linux System Programming" by Robert Love

---

### Thank you 
Contact if you want to join our classes/support: 
[EFXTV](https://t.me/efxtv)

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/efxtv)
