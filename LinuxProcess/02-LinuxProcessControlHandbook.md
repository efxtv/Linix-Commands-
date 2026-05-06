# The Definitive Guide to Stopping and Resuming Processes in Linux

## Signals, Job Control, Process States, and Advanced Process Lifecycle Management

---

## Table of Contents

1. [Introduction to Process Lifecycle Control](#1-introduction-to-process-lifecycle-control)
2. [Linux Process States Related to Stopping and Resuming](#2-linux-process-states-related-to-stopping-and-resuming)
3. [Signals: The Mechanism Behind Process Control](#3-signals-the-mechanism-behind-process-control)
4. [Job Control in the Shell](#4-job-control-in-the-shell)
5. [Terminal-Generated Signals](#5-terminal-generated-signals)
6. [Programmatic Process Control](#6-programmatic-process-control)
7. [The `kill` Command — Complete Reference](#7-the-kill-command--complete-reference)
8. [The `killall`, `pkill`, and `skill` Commands](#8-the-killall-pkill-and-skill-commands)
9. [The `fg`, `bg`, `jobs`, and `wait` Commands](#9-the-fg-bg-jobs-and-wait-commands)
10. [The `disown`, `nohup`, and `setsid` Commands](#10-the-disown-nohup-and-setsid-commands)
11. [Advanced Tools: `strace`, `gdb`, `ptrace`](#11-advanced-tools-strace-gdb-ptrace)
12. [The `/proc` Filesystem for Process Inspection](#12-the-proc-filesystem-for-process-inspection)
13. [C Programming: Controlling Processes Programmatically](#13-c-programming-controlling-processes-programmatically)
14. [Python: Controlling Processes Programmatically](#14-python-controlling-processes-programmatically)
15. [Stopping Processes via cgroups](#15-stopping-processes-via-cgroups)
16. [Freezing Processes via the CGroup Freezer](#16-freezing-processes-via-the-cgroup-freezer)
17. [Checkpoint and Restore (CRIU)](#17-checkpoint-and-restore-criu)
18. [Handling Unstoppable Processes (D State)](#18-handling-unstoppable-processes-d-state)
19. [Practical Scenarios and Recipes](#19-practical-scenarios-and-recipes)
20. [Troubleshooting Common Issues](#20-troubleshooting-common-issues)
21. [Complete Command Reference Table](#21-complete-command-reference-table)
22. [Signal Reference Table](#22-signal-reference-table)
23. [Glossary](#23-glossary)

---

## 1. Introduction to Process Lifecycle Control

In Linux, the ability to stop, pause, resume, and control the execution of processes is one of the most fundamental and powerful features of the operating system. This capability underpins everything from simple interactive shell usage to complex system administration, debugging, container orchestration, and live migration of workloads.

### Why Control Process Execution?

There are numerous reasons to stop or resume a process:

1. **Interactive multitasking**: Switching between tasks in a terminal without terminating running work.
2. **Resource management**: Temporarily pausing CPU-intensive tasks to free resources for higher-priority work.
3. **Debugging**: Halting a process to inspect its state, memory, registers, or stack trace.
4. **System maintenance**: Pausing services during backups, updates, or migrations.
5. **Preventing data loss**: Stopping a runaway process before it consumes all resources.
6. **Job scheduling**: Managing batch workloads that need to yield to interactive work.
7. **Security incident response**: Freezing a suspicious process for forensic analysis.
8. **Live migration**: Checkpointing and resuming processes on different machines.

### The Unix Philosophy of Process Control

Unix provides a layered approach to process control:

- **Signal layer**: The kernel delivers signals to processes, which can catch, ignore, or handle them.
- **Job control layer**: The shell manages foreground/background jobs and process groups.
- **Terminal layer**: The terminal driver generates signals from special keystrokes.
- **User-space tools layer**: Commands like `kill`, `fg`, `bg`, `nohup` provide convenient interfaces.
- **Programmatic layer**: System calls (`kill()`, `raise()`, `pause()`, `sigsuspend()`) allow programs to control themselves and others.
- **Kernel subsystem layer**: cgroup freezer, ptrace, and CRIU provide advanced control mechanisms.

Each layer builds on the ones below it, providing flexibility at every level.

---

## 2. Linux Process States Related to Stopping and Resuming

### 2.1 The Process State Machine

Every process in Linux exists in one of several states at any given moment. Understanding these states is critical to understanding how stopping and resuming work.

```
                    +------------------+
                    |   TASK_RUNNING   |  (R)
                    |  Running/Ready   |
                    +--------+---------+
                             |
              +--------------+--------------+
              |              |              |
      voluntary block   involuntary    exit/terminate
              |           stop                  |
              v              |                  v
+-------------+--+     +-----v------+     +-----+------+
| TASK_INTERRUPTIBLE|  |TASK_STOPPED|     | TASK_DEAD  |
|   Sleeping  (S)   |  |  Stopped(T)|     |  (X/x)     |
+--------+---------+  +-----+------+     +------------+
         |                  |
    event/signal      SIGCONT received
    or signal               |
         |                  v
         |            +-----+------+
         |            | TASK_RUNNING |
         +------------+   (R)        |
                      +-------------+

+------------------+
| TASK_KILLABLE(Sk)| ← Interruptible sleep, but can be killed
+--------+---------+
         |
    fatal signal
         |
         v
   TASK_DEAD
```

### 2.2 Detailed State Descriptions

#### TASK_RUNNING (R) — Running or Runnable

The process is either currently executing on a CPU or is in the runqueue waiting to be scheduled. This is the only state where the process is making progress.

```bash
# Find all running processes
ps -eo pid,stat,comm --no-headers | awk '$2 ~ /^R/'
```

#### TASK_INTERRUPTIBLE (S) — Interruptible Sleep

The process is waiting for an event (I/O completion, a signal, a lock, a timer, etc.). It **can be woken up by signals**, including `SIGCONT`, `SIGKILL`, `SIGTERM`.

**Most common state** for processes waiting on:
- Network I/O (sockets, HTTP requests)
- Disk I/O that can be interrupted
- Sleep/timer waits (`sleep`, `wait`, `select`, `poll`)
- Lock contention (mutexes, semaphores)
- Pipe/FIFO reads when no data available

```bash
# Find all sleeping processes
ps -eo pid,stat,comm --no-headers | awk '$2 ~ /^S/'
```

#### TASK_KILLABLE (SK) — Killable Sleep

Introduced in Linux 2.6.25. This is a variant of interruptible sleep where the process **can only be woken by fatal signals** (like `SIGKILL`). It ignores non-fatal signals while sleeping.

Used in critical kernel paths (NFS mounts, DLM locks) where `TASK_UNINTERRUPTIBLE` caused unkillable processes but `TASK_INTERRUPTIBLE` was too permissive.

```bash
# Find killable-sleep processes (shown as 'S' with special flags in newer kernels)
ps -eo pid,stat,comm --no-headers | grep -i 'sk\|S<'
```

#### TASK_UNINTERRUPTIBLE (D) — Uninterruptible Sleep

The process is waiting for I/O and **cannot be interrupted by any signal** — not even `SIGKILL`. This state exists to protect kernel data structures from corruption during critical I/O operations.

**Common causes:**
- NFS server is unreachable
- Disk hardware failure
- Slow storage device
- Waiting for device driver response

```bash
# Find uninterruptible sleep processes (often problematic)
ps -eo pid,stat,comm --no-headers | awk '$2 ~ /^D/'

# Show what they're waiting for (kernel stack)
cat /proc/<pid>/stack
```

**Important:** You CANNOT stop or kill a process in D state. You must fix the underlying I/O issue or reboot.

#### TASK_STOPPED (T) — Stopped

The process has been **explicitly stopped** by receiving `SIGSTOP`, `SIGTSTP`, `SIGTTIN`, or `SIGTTOU`. The process is not scheduled for execution. It retains all its resources and memory.

**To resume:** Send `SIGCONT`.

```bash
# Find all stopped processes
ps -eo pid,stat,comm --no-headers | awk '$2 ~ /^T/'
```

#### TASK_TRACED (t) — Traced

The process is being **debugged** (via `ptrace`) and has been stopped by the debugger. Similar to TASK_STOPPED but caused by ptrace rather than job control signals.

**Triggers:**
- Breakpoints set by `gdb`
- `strace` intercepting syscalls
- Single-stepping with debugger

```bash
# Find traced processes
ps -eo pid,stat,comm --no-headers | awk '$2 ~ /^t/'
```

#### TASK_DEAD (X) — Dead

The process is being removed from the system. This state is transient and should never be visible in `ps` output.

#### TASK_IDLE (I) — Idle Kernel Thread

Kernel thread is idle. Introduced in Linux 4.14. These processes are excluded from the load average calculation.

```bash
# Show idle kernel threads
ps -eo pid,stat,comm --no-headers | awk '$2 ~ /^I/'
```

### 2.3 State Modifiers in `ps` Output

| Modifier | Meaning | Example |
|---|---|---|
| `<` | High priority (nice < 0) | `R<` |
| `N` | Low priority (nice > 0) | `SN` |
| `s` | Session leader | `Ss` |
| `l` | Multi-threaded | `Sl` |
| `L` | Has pages locked in memory | `SL` |
| `+` | Foreground process group | `S+` |

**Combined examples:**
- `Ss+` = Sleeping, session leader, foreground (e.g., your current shell)
- `SNl` = Sleeping, low priority, multi-threaded
- `R<` = Running, high priority
- `Tl` = Stopped, multi-threaded
- `Sl` = Sleeping, multi-threaded (most daemons)

---

## 3. Signals: The Mechanism Behind Process Control

### 3.1 What Are Signals?

Signals are a form of **inter-process communication (IPC)** in Linux. They are asynchronous notifications sent to a process (or process group) to notify it of an event. Signals are the primary mechanism for stopping, resuming, and terminating processes.

### 3.2 Signal Categories

Signals are divided into categories based on their behavior and origin:

#### Category 1: Terminal-Generated Signals

| Signal | Generated By | Default Action |
|---|---|---|
| `SIGINT` (2) | Ctrl+C | Terminate |
| `SIGQUIT` (3) | Ctrl+\ | Terminate + core dump |
| `SIGTSTP` (20) | Ctrl+Z | Stop |
| `SIGTTIN` (26) | Background process reads from terminal | Stop |
| `SIGTTOU` (27) | Background process writes to terminal | Stop |
| `SIGHUP` (1) | Terminal disconnect / hangup | Terminate |
| `SIGWINCH` (28) | Terminal window resized | Ignore |

#### Category 2: Process Control Signals

| Signal | Purpose | Default Action | Can Be Caught? |
|---|---|---|---|
| `SIGSTOP` (19) | Stop process immediately | Stop | **No** |
| `SIGCONT` (18) | Resume a stopped process | Continue | **Yes** (but default action always occurs) |
| `SIGKILL` (9) | Force kill process | Terminate | **No** |

#### Category 3: Termination Signals

| Signal | Purpose | Default Action |
|---|---|---|
| `SIGTERM` (15) | Graceful termination | Terminate |
| `SIGINT` (2) | Interrupt | Terminate |
| `SIGQUIT` (3) | Quit with core dump | Core dump |
| `SIGHUP` (1) | Hangup | Terminate |
| `SIGABRT` (6) | Abort (from `abort()`) | Core dump |

#### Category 4: Error/Exception Signals

| Signal | Cause | Default Action |
|---|---|---|
| `SIGSEGV` (11) | Invalid memory access (segmentation fault) | Core dump |
| `SIGBUS` (7) | Bus error (invalid memory alignment) | Core dump |
| `SIGFPE` (8) | Floating-point exception (divide by zero) | Core dump |
| `SIGILL` (4) | Illegal instruction | Core dump |
| `SIGTRAP` (5) | Trap/breakpoint (debugger) | Core dump |
| `SIGSYS` (31) | Bad system call | Core dump |

#### Category 5: Resource and Timer Signals

| Signal | Cause | Default Action |
|---|---|---|
| `SIGALRM` (14) | Timer expired (`alarm()`) | Terminate |
| `SIGVTALRM` (26) | Virtual timer expired | Terminate |
| `SIGPROF` (27) | Profiling timer expired | Terminate |
| `SIGXCPU` (24) | CPU time limit exceeded | Core dump |
| `SIGXFSZ` (25) | File size limit exceeded | Core dump |
| `SIGPIPE` (13) | Write to pipe with no reader | Terminate |
| `SIGUSR1` (10) | User-defined signal 1 | Terminate |
| `SIGUSR2` (12) | User-defined signal 2 | Terminate |

#### Category 6: Job Control Signals

| Signal | Cause | Default Action |
|---|---|---|
| `SIGCHLD` (17) | Child process stopped or terminated | Ignore |
| `SIGCLD` | Same as SIGCHLD (System V) | Ignore |

### 3.3 Signal Disposition

Every signal has a **disposition** — what the kernel does when the signal is delivered:

1. **Default action**: The kernel's built-in behavior (terminate, stop, continue, ignore, core dump).
2. **Caught**: The process has registered a signal handler function via `signal()` or `sigaction()`.
3. **Ignored**: The process explicitly set the signal to `SIG_IGN` — the kernel discards it.
4. **Blocked**: The signal is in the process's signal mask — delivery is deferred until unblocked.

### 3.4 Signals That Cannot Be Caught or Ignored

Two signals are special:

| Signal | Number | Why Uncatchable |
|---|---|---|
| `SIGKILL` | 9 | Guarantees that the kernel can always terminate any process. If it could be caught, a buggy or malicious process could become immortal. |
| `SIGSTOP` | 19 | Guarantees that the kernel (or a user) can always stop any process for debugging or job control. |

```bash
# These will silently fail — no error, but no effect either
kill -KILL <pid>   # Works (SIGKILL cannot be caught, so it always terminates)
kill -STOP <pid>   # Works (SIGSTOP cannot be caught, so it always stops)

# But a process cannot register a handler for them:
# signal(SIGKILL, handler)  ← silently ignored by kernel
# signal(SIGSTOP, handler)  ← silently ignored by kernel
```

### 3.5 SIGCONT — The Special Resume Signal

`SIGCONT` has unique properties:

1. **Cannot be blocked**: Even if `SIGCONT` is in the signal mask, it is always delivered.
2. **Default action always occurs**: Even if a handler is registered, the process is always resumed first.
3. **Handler runs after resume**: If a handler is registered, it runs after the process has been resumed.
4. **Idempotent**: Sending `SIGCONT` to a running process has no effect (and is not an error).

```bash
# Resume any stopped process (works even if process isn't stopped)
kill -CONT <pid>

# Resume entire process group
kill -CONT -<pgid>
```

### 3.6 Signal Delivery and Queuing

**Standard signals** (1-31):
- **Not queued**: If the same signal is sent multiple times while blocked, only one instance is delivered.
- Ordering between different signals is not guaranteed.

**Real-time signals** (34-64, `SIGRTMIN` to `SIGRTMAX`):
- **Queued**: Multiple instances are delivered in order.
- Carry an integer value (`sigval_t`).
- Sent via `sigqueue()`.

```bash
# List all real-time signals available
kill -l | tr ' ' '\n' | grep RT
```

### 3.6.1 Signal Masks and Blocking

A process can **block** signals (defer delivery) using its signal mask:

```bash
# View pending (blocked but not yet delivered) signals for a process
cat /proc/<pid>/status | grep -i sig

# Fields:
# SigPnd: Signals pending for the thread
# SigBlk: Signals currently blocked
# SigIgn: Signals being ignored
# SigCgt: Signals being caught
```

The values are bitmasks in hexadecimal. To decode:

```bash
# Convert hex mask to signal names
python3 -c "
mask = 0x0000000000000004
for i in range(1, 32):
    if mask & (1 << (i-1)):
        print(f'Signal {i} is set')
"
```

### 3.7 Signal Safety

When a signal handler runs, it **interrupts** the normal flow of execution. This creates constraints:

**Async-signal-safe functions** (can be called from signal handlers):
- `_exit()`, `signal()`, `sigaction()`, `kill()`, `raise()`, `pause()`
- `read()`, `write()`, `fcntl()`, `select()` (with caveats)
- `wait()`, `waitpid()`, `alarm()`
- See `man 7 signal-safety` for the complete list

**NOT safe in signal handlers:**
- `printf()`, `malloc()`, `free()`, `exit()`
- Most standard library functions

---

## 4. Job Control in the Shell

### 4.1 What Is Job Control?

Job control is a **shell feature** that allows users to manage multiple processes (jobs) from a single terminal session. It was introduced in the C shell (csh) in the late 1970s and is now standard in all modern shells (bash, zsh, fish, dash with limitations).

A **job** is a pipeline of one or more processes that the shell manages as a unit. For example:

```bash
$ find / -name "*.log" | grep error | sort
```

This is **one job** consisting of three processes (`find`, `grep`, `sort`) in a single process group.

### 4.2 How Job Control Works Internally

1. The shell creates a **process group** for each job using `setpgid()`.
2. The shell tracks jobs in a **job table** (internal data structure).
3. The shell uses **terminal foreground/background** control via `tcsetpgrp()`.
4. The shell receives `SIGCHLD` when child processes change state.
5. The shell maps signals to job operations (Ctrl+Z → `SIGTSTP`, Ctrl+C → `SIGINT`).

### 4.3 Enabling and Checking Job Control

```bash
# Check if job control is enabled
set -o | grep monitor
# monitor           on    ← enabled

# Enable job control
set -m
# or
set -o monitor

# Disable job control
set +m
# or
set +o monitor

# Job control is always enabled in interactive shells
# It is disabled by default in non-interactive (script) shells
```

### 4.4 Job Identification

Jobs are identified in two ways:

**Job Specification (jobspec):**
| Spec | Meaning |
|---|---|
| `%n` | Job number n |
| `%` or `%+` or `%%` | Current job (most recently used) |
| `%-` | Previous job |
| `%string` | Job whose command begins with "string" |
| `%?string` | Job whose command contains "string" |

**Example:**
```bash
$ sleep 100 &
[1] 5001
$ vim file.txt &
[2] 5002
$ find / -name "*.conf" | grep ssl &
[3] 5003

# Reference by job number
fg %1        # Bring job 1 to foreground
fg %+        # Bring current job (job 3) to foreground
fg %-        # Bring previous job (job 2) to foreground
fg %vim      # Bring job starting with "vim" to foreground
fg %?conf    # Bring job containing "conf" to foreground
```

---

## 5. Terminal-Generated Signals

### 5.1 Special Keystrokes and Their Signals

| Keystroke | Signal Sent To | Effect |
|---|---|---|
| `Ctrl+C` | Foreground process group | `SIGINT` — interrupt/terminate |
| `Ctrl+Z` | Foreground process group | `SIGTSTP` — stop/suspend |
| `Ctrl+\` | Foreground process group | `SIGQUIT` — quit + core dump |
| `Ctrl+D` | Shell (not a signal) | EOF on stdin — exits shell if line is empty |
| `Ctrl+Y` | Terminal | Delayed suspend (not always supported) |

### 5.2 Configuring Terminal Control Characters

```bash
# View current terminal settings
stty -a

# Key lines to look for:
# intr = ^C;    quit = ^\;    erase = ^?;    kill = ^U;
# eof = ^D;     start = ^Q;   stop = ^S;     susp = ^Z;
# rprnt = ^R;   werase = ^W;  lnext = ^V;    flush = ^O;

# Change control characters
stty intr ^G          # Use Ctrl+G instead of Ctrl+C for interrupt
stty susp ^Y          # Use Ctrl+Y instead of Ctrl+Z for suspend
stty quit ^]          # Use Ctrl+] instead of Ctrl+\ for quit
stty stop ^S          # XOFF (stop output)
stty start ^Q         # XON (resume output)

# Disable a control character
stty susp undef       # Disable Ctrl+Z suspend
stty intr undef       # Disable Ctrl+C interrupt
```

### 5.3 XON/XOFF Flow Control

`Ctrl+S` stops terminal output (XOFF), `Ctrl+Q` resumes it (XON). This is **NOT** process control — it only pauses the terminal display.

```bash
# Disable XON/XOFF (often accidentally triggered)
stty -ixon

# Re-enable
stty ixon
```

### 5.4 Terminal Process Group Management

```bash
# Find the foreground process group of a terminal
# The terminal stores the foreground PGID
cat /proc/self/stat | awk '{print $5}'

# Set foreground process group (requires appropriate permissions)
# This is done by the shell via tcsetpgrp()
```

---

## 6. Programmatic Process Control

### 6.1 System Calls Overview

| System Call | Purpose | Header |
|---|---|---|
| `kill(pid, sig)` | Send signal to process or process group | `<signal.h>` |
| `killpg(pgid, sig)` | Send signal to process group | `<signal.h>` |
| `raise(sig)` | Send signal to calling process | `<signal.h>` |
| `pause()` | Suspend until any signal arrives | `<unistd.h>` |
| `sigsuspend(mask)` | Replace mask and pause atomically | `<signal.h>` |
| `sigaction(signo, act, oldact)` | Set signal disposition | `<signal.h>` |
| `sigprocmask(how, set, oldset)` | Modify signal mask | `<signal.h>` |
| `sigpending(set)` | Check pending signals | `<signal.h>` |
| `sigtimedwait(set, info, timeout)` | Wait for specific signals | `<signal.h>` |
| `sigwait(set, sig)` | Wait synchronously for signals | `<signal.h>` |
| `sigqueue(pid, sig, value)` | Send signal with data | `<signal.h>` |
| `alarm(seconds)` | Set SIGALRM timer | `<unistd.h>` |
| `setitimer(which, value, oldvalue)` | Set interval timers | `<sys/time.h>` |
| `timer_create(clockid, sev, timerid)` | POSIX timers | `<time.h>` |
| `signalfd(fd, mask, flags)` | File descriptor for signals | `<sys/signalfd.h>` |
| `waitpid(pid, status, options)` | Wait for child state change | `<sys/wait.h>` |
| `waitid(idtype, id, infop, options)` | Wait with more detail | `<sys/wait.h>` |
| `ptrace(request, pid, addr, data)` | Trace/inspect/modify process | `<sys/ptrace.h>` |
| `setpgid(pid, pgid)` | Set process group | `<unistd.h>` |
| `setsid()` | Create new session | `<unistd.h>` |
| `tcsetpgrp(fd, pgrp)` | Set terminal foreground PGID | `<termios.h>` |
| `tcgetpgrp(fd)` | Get terminal foreground PGID | `<termios.h>` |

### 6.2 Signal Handler Registration

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

volatile sig_atomic_t got_signal = 0;

void handler(int sig) {
    got_signal = 1;
    // Only use async-signal-safe functions here
    write(STDOUT_FILENO, "Signal received!\n", 19);
}

int main() {
    struct sigaction sa;
    sa.sa_handler = handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;  // Or SA_RESTART to auto-restart interrupted syscalls

    sigaction(SIGINT, &sa, NULL);
    sigaction(SIGTERM, &sa, NULL);

    // Ignore SIGQUIT
    signal(SIGQUIT, SIG_IGN);

    // Default action for SIGUSR1
    signal(SIGUSR1, SIG_DFL);

    while (1) {
        pause();  // Sleep until a signal arrives
        if (got_signal) {
            printf("Resuming work...\n");
            got_signal = 0;
        }
    }

    return 0;
}
```

### 6.3 Signal Masking (Blocking Signals)

```c
#include <signal.h>

int main() {
    sigset_t set, oldset;

    // Initialize an empty set
    sigemptyset(&set);

    // Add signals to block
    sigaddset(&set, SIGINT);
    sigaddset(&set, SIGTERM);
    sigaddset(&set, SIGQUIT);

    // Block the signals
    sigprocmask(SIG_BLOCK, &set, &oldset);

    // Critical section — signals are deferred
    // ... do work ...

    // Restore previous mask (signals are delivered now)
    sigprocmask(SIG_SETMASK, &oldset, NULL);

    return 0;
}
```

### 6.4 Waiting for Signals Synchronously

```c
#include <signal.h>

int main() {
    sigset_t set;
    int sig;

    sigemptyset(&set);
    sigaddset(&set, SIGINT);
    sigaddset(&set, SIGTERM);

    // Block signals first
    sigprocmask(SIG_BLOCK, &set, NULL);

    // Now wait for one of them
    while (1) {
        sigwait(&set, &sig);

        if (sig == SIGINT) {
            printf("Caught SIGINT\n");
        } else if (sig == SIGTERM) {
            printf("Caught SIGTERM, exiting gracefully\n");
            break;
        }
    }

    return 0;
}
```

---

## 7. The `kill` Command — Complete Reference

### 7.1 Basic Usage

```bash
kill [options] <pid> [...]
kill [options] <pid> [...]
```

Despite its name, `kill` **sends signals** — it doesn't just kill processes.

### 7.2 Signal Specification Methods

```bash
# By signal number
kill -9 1234
kill -19 1234
kill -18 1234

# By signal name (with or without SIG prefix)
kill -KILL 1234
kill -SIGKILL 1234
kill -STOP 1234
kill -SIGSTOP 1234
kill -CONT 1234
kill -SIGCONT 1234
kill -TERM 1234
kill -SIGTERM 1234
kill -TSTP 1234
kill -SIGTSTP 1234

# Short names
kill -INT 1234
kill -HUP 1234
kill -QUIT 1234
kill -ABRT 1234
kill -USR1 1234
kill -USR2 1234
kill -CHLD 1234

# By number (all equivalent)
kill -2 1234    # SIGINT
kill -15 1234   # SIGTERM
kill -19 1234   # SIGSTOP
kill -18 1234   # SIGCONT
```

### 7.3 Listing Available Signals

```bash
# List all signals (short names)
kill -l

# List all signals (numbered)
kill -L

# List real-time signals
kill -l RTMIN
kill -l RTMAX
```

### 7.4 Target Specifications

```bash
# Send to specific PIDs
kill -STOP 1234 5678 9012

# Send to process group (negative PID)
kill -STOP -1234   # Send to process group 1234

# Send to all processes (requires root)
kill -STOP -1      # Send to all processes with PID > 0
kill -KILL -1      # Kill all processes (dangerous!)

# Send to processes owned by current user
kill -STOP -u $USER

# Send to all processes (special syntax)
killall -STOP -u username
```

### 7.5 Stopping Processes with `kill`

```bash
# Stop a process (SIGSTOP — cannot be caught)
kill -STOP <pid>
kill -19 <pid>
kill -SIGSTOP <pid>

# Stop entire process group
kill -STOP -<pgid>
kill -- -<pgid>

# Stop by name (requires killall or pkill)
killall -STOP <name>
pkill -STOP <name>
```

### 7.6 Resuming Processes with `kill`

```bash
# Resume a stopped process
kill -CONT <pid>
kill -18 <pid>
kill -SIGCONT <pid>

# Resume entire process group
kill -CONT -<pgid>

# Resume by name
killall -CONT <name>
pkill -CONT <name>
```

### 7.7 Graceful Termination

```bash
# Send SIGTERM (default, graceful)
kill <pid>
kill -TERM <pid>
kill -15 <pid>

# Wait, then force kill if still running
kill <pid>
sleep 5
kill -9 <pid>    # Force kill after 5 seconds

# Send HUP to request reload (many daemons support this)
kill -HUP <pid>
kill -1 <pid>
# Common for: nginx, sshd, syslog, apache
```

### 7.8 Checking if Process Exists

```bash
# Signal 0 does nothing but check if process exists and you can signal it
kill -0 <pid>
echo $?   # 0 = exists, 1 = doesn't exist or no permission

# In scripts
if kill -0 "$PID" 2>/dev/null; then
    echo "Process $PID is running"
else
    echo "Process $PID is not running"
fi
```

### 7.9 `kill` Options

```bash
# Show usage
kill --help

# Signal number (-L lists with names)
kill -L

# Verbose (some implementations)
kill -v -STOP <pid>
```

### 7.10 Practical `kill` Examples

```bash
# Stop a misbehaving process for investigation
kill -STOP $(pgrep firefox)
# Now inspect /proc/<pid>/... without the process changing

# Resume it
kill -CONT $(pgrep firefox)

# Stop all child processes of a parent
for pid in $(pgrep -P <parent_pid>); do
    kill -STOP $pid
done

# Stop a job by jobspec (in bash)
kill -STOP %1

# Resume a job by jobspec
kill -CONT %1

# Stop process and all its descendants
pstree -p <pid> | grep -oP '\d+' | xargs kill -STOP

# Resume process tree
pstree -p <pid> | grep -oP '\d+' | xargs kill -CONT

# Toggle stop/resume (if stopped, resumes; if running, stops)
# Note: There's no single toggle signal; you must check state first
state=$(ps -o stat= -p <pid>)
if [[ $state == *T* ]]; then
    kill -CONT <pid>
else
    kill -STOP <pid>
fi
```

---

## 8. The `killall`, `pkill`, and `skill` Commands

### 8.1 `killall` — Kill Processes by Name

```bash
# Basic usage
killall <name>

# Send specific signal
killall -STOP <name>
killall -CONT <name>
killall -TERM <name>
killall -KILL <name>
killall -HUP <name>

# By signal number
killall -19 <name>   # STOP
killall -18 <name>   # CONT
killall -15 <name>   # TERM

# Interactive (ask for confirmation)
killall -i <name>

# Quiet (no error if process not found)
killall -q <name>

# Wait for processes to die
killall -w <name>

# Kill processes older than X time
killall -o 1h <name>     # Older than 1 hour
killall -o 30m <name>    # Older than 30 minutes
killall -o 2d <name>     # Older than 2 days

# Kill processes younger than X time
killall -y 5m <name>     # Younger than 5 minutes

# Kill by exact name (don't match substrings)
killall -e <name>

# Case-insensitive matching
killall -I <name>

# Kill processes owned by specific user
killall -u username <name>

# Kill processes in specific terminal
killall -t pts/0 <name>

# List matching processes without killing
killall -l <name>        # Not all versions support this

# Verbose output
killall -v <name>

# Regular expression matching (pkill)
pkill -STOP "^myapp.*"
```

**Examples:**
```bash
# Stop all nginx worker processes
killall -STOP nginx

# Resume all python processes
killall -CONT python3

# Gracefully reload nginx
killall -HUP nginx

# Kill all user's processes
killall -u john

# Stop all processes owned by www-data
killall -STOP -u www-data
```

### 8.2 `pkill` — Kill Processes by Pattern

`pkill` is more flexible than `killall` — it matches against the full command line, not just the process name.

```bash
# Basic usage
pkill <pattern>

# Send specific signal
pkill -STOP <pattern>
pkill -CONT <pattern>
pkill -TERM <pattern>
pkill -KILL <pattern>

# Match against full command line
pkill -f "python server.py"

# Case-insensitive
pkill -i <pattern>

# Exact match (match entire name)
pkill -x <pattern>

# Match oldest process only
pkill -o <pattern>

# Match newest process only
pkill -n <pattern>

# Match by terminal
pkill -t pts/0 <pattern>

# Match by user
pkill -u username <pattern>
pkill -U username <pattern>    # Real UID

# Match by group
pkill -G groupname <pattern>
pkill -g pgid <pattern>        # Process group ID

# Match by session
pkill -s sid <pattern>

# Show what would be matched (dry run)
pkill -c <pattern>              # Count matching processes

# Verbose
pkill -v <pattern>

# List matching processes
pgrep -a <pattern>

# Invert match (kill everything EXCEPT matching)
pkill -v <pattern>

# Signal by number
pkill -19 <pattern>
pkill -18 <pattern>

# Match against parent PID
pkill -P <ppid>

# Match process group
pkill -g <pgid>
```

**Examples:**
```bash
# Stop all processes running a specific script
pkill -STOP -f "backup.sh"

# Resume all processes containing "worker" in command line
pkill -CONT -f "worker"

# Stop all child processes of a specific parent
pkill -STOP -P 1234

# Kill all processes in a specific process group
pkill -TERM -g 5678

# Stop all processes on a specific terminal
pkill -STOP -t pts/1

# Find and stop Java processes
pkill -STOP -f "java .*myapp"

# Kill all processes except sshd
pkill -TERM -v sshd    # DANGER: be careful with this!
```

### 8.3 `pgrep` — Find Process PIDs

`pgrep` is the companion to `pkill` — it finds PIDs without sending signals.

```bash
# Find PID by name
pgrep <name>

# Show full command line
pgrep -a <name>

# Show process list format
pgrep -l <name>

# Count matching processes
pgrep -c <name>

# List full command line
pgrep -f <pattern>

# Case-insensitive
pgrep -i <name>

# Exact match
pgrep -x <name>

# Newest/oldest
pgrep -n <name>
pgrep -o <name>

# By parent
pgrep -P <ppid>

# By terminal
pgrep -t pts/0

# By user
pgrep -u username

# Delimiter for output
pgrep -d ',' <name>    # Comma-separated PIDs

# List processes with full details
pgrep -a -u www-data nginx
```

### 8.4 `skill` and `snice` (Legacy)

`skill` and `snice` are part of the `psmisc` package and are largely superseded by `pkill`.

```bash
# Send signal to processes (equivalent to pkill)
skill -STOP <name>
skill -CONT <name>
skill -KILL <name>

# Send signal to specific user's processes
skill -STOP -u username

# Send signal to processes on specific terminal
skill -STOP -t pts/0

# Change priority (like renice)
snice +5 -u username
snice -10 -t pts/0
```

---

## 9. The `fg`, `bg`, `jobs`, and `wait` Commands

### 9.1 `jobs` — List Background Jobs

```bash
# List all jobs
jobs

# List with PIDs
jobs -l
jobs -p

# List only running jobs
jobs -r

# List only stopped jobs
jobs -s

# Show process group IDs
jobs -x <command>

# Verbose output
jobs -v
```

**Output explanation:**
```bash
$ jobs -l
[1]  + 5001 suspended (tty output)  vim file.txt
[2]  - 5002 running                 sleep 300 &
[3]    5003 stopped                  find / -name "*.conf"
    ^     ^        ^                   ^
    |     |        |                   |
 job#   PID    status              command
  |
  + = current job (used by %)
  - = previous job
```

### 9.2 `fg` — Bring Job to Foreground

```bash
# Bring current job to foreground
fg

# Bring specific job to foreground
fg %1
fg %2
fg %3

# Bring job by name prefix
fg %vim
fg %sleep

# Bring job by name pattern
fg %?find

# The shell will:
# 1. Send SIGCONT to the job's process group
# 2. Make the job's process group the foreground process group of the terminal
# 3. Wait for the job to complete or stop again
```

**What happens internally:**
```bash
# Equivalent to what fg does internally:
kill -CONT %1                  # Resume the job
tcsetpgrp(tty, getpgrp(%1))   # Set as foreground process group
waitpid(%1, &status, WUNTRACED) # Wait for state change
```

### 9.3 `bg` — Resume Job in Background

```bash
# Resume current stopped job in background
bg

# Resume specific job in background
bg %1
bg %2

# The shell will:
# 1. Send SIGCONT to the job's process group
# 2. Keep the job in the background (not the foreground process group)
# 3. The job runs concurrently with the shell
```

**Example workflow:**
```bash
$ vim large_file.txt          # Running in foreground
# (Press Ctrl+Z)
[1]+  Stopped    vim large_file.txt

$ ls -la                      # Do other work

$ bg                          # Resume vim in background
[1]+ vim large_file.txt &

$ fg                          # Bring it back to foreground
[1]+ vim large_file.txt
```

### 9.4 `wait` — Wait for Job Completion

```bash
# Wait for all background jobs to complete
wait

# Wait for specific job
wait %1
wait <pid>

# Wait and return exit status
wait %1
echo $?

# Wait for any one job to complete (bash 4.3+)
wait -n

# Wait for any job and print its PID and status (bash 5.0+)
wait -n -p VARNAME
echo "Job $VARNAME completed with status: $?"

# Wait with specific job control options
wait -f <pid>   # Wait until process actually terminates (not just state change)
```

**Example:**
```bash
#!/bin/bash
# Start multiple background jobs
sleep 5 &
pid1=$!

sleep 10 &
pid2=$!

sleep 3 &
pid3=$!

# Wait for all to complete
wait $pid1 $pid2 $pid3
echo "All jobs completed"

# Wait for any single job (bash 4.3+)
wait -n
echo "First job completed"
```

### 9.5 Interactive Job Control Workflow

```bash
# 1. Start a process in foreground
$ python3 server.py
# (Server is running, terminal blocked)

# 2. Suspend it with Ctrl+Z
^Z
[1]+  Stopped    python3 server.py

# 3. List jobs
$ jobs -l
[1]+  12345 Suspended    python3 server.py

# 4. Resume in background
$ bg %1
[1]+ python3 server.py &

# 5. Check it's running
$ jobs -l
[1]+  12345 Running    python3 server.py &

# 6. Bring back to foreground when needed
$ fg %1
python3 server.py

# 7. Or stop it from background
$ kill -STOP %1

# 8. Or kill it from background
$ kill %1
# or
$ kill -9 %1
```

---

## 10. The `disown`, `nohup`, and `setsid` Commands

### 10.1 `disown` — Remove Job from Shell's Job Table

`disown` removes a job from the shell's job table, preventing the shell from sending `SIGHUP` to it when the shell exits.

```bash
# Disown the current job
disown

# Disown specific job
disown %1
disown %2

# Disown by PID
disown -h <pid>

# Disown all jobs
disown -a

# Disown only running jobs
disown -r

# Remove from job table AND prevent SIGHUP
disown %1

# Note: disown -h only prevents SIGHUP but keeps in job table
#       disown (without -h) removes from job table entirely
```

**Example:**
```bash
$ long_running_task &
[1] 12345

$ disown %1
# Now the shell won't send SIGHUP when it exits
# Job 1 is no longer listed in 'jobs'

# The process will survive terminal closure
```

### 10.2 `nohup` — Run Command Immune to Hangups

`nohup` runs a command that ignores `SIGHUP` and redirects output to `nohup.out`.

```bash
# Basic usage
nohup <command> &

# Redirect output manually
nohup <command> > output.log 2>&1 &

# Redirect to /dev/null
nohup <command> > /dev/null 2>&1 &

# Append to file
nohup <command> >> output.log 2>&1 &

# With nice (low priority, nohup)
nohup nice -n 19 <command> > output.log 2>&1 &
```

**What `nohup` does internally:**
1. Sets `SIGHUP` disposition to `SIG_IGN`.
2. Redirects stdout to `nohup.out` (if stdout is a terminal).
3. Redirects stderr to stdout (if stderr is a terminal).
4. Executes the command.

```bash
# Check if a process is immune to SIGHUP
cat /proc/<pid>/status | grep SigIgn
# If bit 0 (SIGHUP = signal 1) is set, it ignores SIGHUP
```

**Combined with disown:**
```bash
# Start, background, and disown in one go
long_command > output.log 2>&1 &
disown

# Or use nohup (which handles SIGHUP automatically)
nohup long_command > output.log 2>&1 &
```

### 10.3 `setsid` — Run Command in New Session

`setsid` runs a command in a **new session**, completely detached from the current terminal.

```bash
# Run in new session
setsid <command>

# Run in new session with output redirection
setsid <command> > output.log 2>&1 &

# Check that process is in new session
ps -o pid,sid,tty,comm -p <pid>
# TTY should be '?' (no terminal)

# From a script
setsid bash -c 'exec <command>' </dev/null >/dev/null 2>&1 &
```

**What `setsid` does:**
1. Calls `fork()` — parent exits.
2. Child calls `setsid()` — creates new session, becomes session leader.
3. Child has no controlling terminal.
4. Child will NOT receive `SIGHUP` from the original terminal.

**Comparison:**

| Method | Survives Terminal Close | Has TTY | In Job Table |
|---|---|---|---|
| `&` (background) | No (gets SIGHUP) | Yes | Yes |
| `nohup cmd &` | Yes (ignores SIGHUP) | Yes | Yes |
| `disown %1` | Yes | Yes | No |
| `setsid cmd` | Yes (new session) | No | No |
| `cmd &` then `disown` | Yes | Yes | No |
| `systemd` service | Yes | No | No |

---

## 11. Advanced Tools: `strace`, `gdb`, `ptrace`

### 11.1 `strace` — Trace System Calls

`strace` uses `ptrace` to intercept and log all system calls made by a process. It can also send signals to the traced process.

```bash
# Trace a new command
strace <command>

# Trace a running process
strace -p <pid>

# Trace with timestamps
strace -t -p <pid>
strace -tt -p <pid>     # Microsecond precision
strace -ttt -p <pid>    # Unix epoch timestamps

# Trace specific syscalls
strace -e trace=signal -p <pid>        # Only signal-related syscalls
strace -e trace=read,write -p <pid>    # Only read/write
strace -e trace=process -p <pid>       # Process-related (fork, exec, kill)
strace -e trace=network -p <pid>       # Network syscalls
strace -e trace=file -p <pid>          # File-related syscalls

# Trace signal delivery
strace -e signal=all -p <pid>

# Trace with signal injection
strace -e inject=kill:error=EPERM -p <pid>

# Trace child processes
strace -f <command>
strace -f -p <pid>

# Follow forks
strace -ff -p <pid>    # Separate output file per process

# Limit output
strace -c -p <pid>     # Summary/counts only (run, then Ctrl+C)

# Show time spent in each syscall
strace -T -p <pid>

# Show syscall arguments in verbose form
strace -v -p <pid>

# Trace with signal stopping
strace -e signal=SIGSTOP -p <pid>

# Deliver a signal to traced process
# (strace intercepts signals; use -e to control)
```

### 11.2 `gdb` — GNU Debugger

`gdb` can attach to running processes, stop them, inspect state, and resume them.

```bash
# Attach to running process
gdb -p <pid>

# Attach and immediately stop
gdb --args <program>

# Once in gdb:

# Stop the process (it's already stopped when attached)
# (gdb stops the process automatically on attach)

# Continue execution
(gdb) continue
(gdb) c

# Continue until next signal
(gdb) handle SIGSTOP stop
(gdb) handle SIGCONT nostop

# Step one instruction
(gdb) stepi
(gdb) si

# Step one line of source
(gdb) step
(gdb) s

# Next line (don't step into functions)
(gdb) next
(gdb) n

# Set breakpoint
(gdb) break main
(gdb) break file.c:42
(gdb) break function_name

# Run until breakpoint
(gdb) run

# Inspect state when stopped
(gdb) info registers
(gdb) info threads
(gdb) backtrace
(gdb) bt
(gdb) print variable
(gdb) p variable
(gdb) info locals

# Send signal to process
(gdb) signal SIGCONT
(gdb) signal SIGSTOP
(gdb) signal SIGTERM

# Detach (resume process)
(gdb) detach

# Detach and kill
(gdb) kill

# Quit gdb
(gdb) quit
(gdb) q
```

**Batch mode (non-interactive):**
```bash
# Attach, run commands, detach
gdb -batch -p <pid> -ex "info registers" -ex "bt" -ex "detach"

# Attach, set breakpoint, continue
gdb -batch -p <pid> -ex "break function" -ex "continue"

# Generate core dump
gdb -batch -p <pid> -ex "generate-core-file" -ex "detach"
```

### 11.3 `ptrace` — The Underlying Mechanism

`ptrace` is the system call that powers `strace`, `gdb`, and other debugging tools.

```c
#include <sys/ptrace.h>

long ptrace(enum __ptrace_request request, pid_t pid,
            void *addr, void *data);
```

**Common requests:**

| Request | Purpose |
|---|---|
| `PTRACE_ATTACH` | Attach to process (stops it) |
| `PTRACE_DETACH` | Detach from process (resumes it) |
| `PTRACE_CONT` | Continue stopped process |
| `PTRACE_SINGLESTEP` | Single-step one instruction |
| `PTRACE_SYSCALL` | Continue until next syscall entry/exit |
| `PTRACE_PEEKTEXT` | Read memory from traced process |
| `PTRACE_POKETEXT` | Write memory to traced process |
| `PTRACE_GETREGS` | Read register state |
| `PTRACE_SETREGS` | Write register state |
| `PTRACE_SETOPTIONS` | Set tracing options (follow forks, etc.) |
| `PTRACE_GETEVENTMSG` | Get event message (forked child PID, etc.) |
| `PTRACE_INTERRUPT` | Interrupt a stopped tracee |
| `PTRACE_LISTEN` | Restart tracee to wait for signals |

**Simple ptrace example:**
```c
#include <sys/ptrace.h>
#include <sys/wait.h>
#include <sys/user.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]) {
    pid_t pid = atoi(argv[1]);

    // Attach to process
    if (ptrace(PTRACE_ATTACH, pid, NULL, NULL) == -1) {
        perror("ptrace attach failed");
        return 1;
    }

    // Wait for process to stop
    waitpid(pid, NULL, 0);
    printf("Process %d is now stopped\n");

    // Read registers
    struct user_regs_struct regs;
    ptrace(PTRACE_GETREGS, pid, NULL, &regs);
    printf("Instruction pointer: %llx\n", regs.rip);

    // Resume execution
    ptrace(PTRACE_DETACH, pid, NULL, NULL);
    printf("Process %d detached and resumed\n");

    return 0;
}
```

**ptrace limitations:**
- Only one tracer per process (`ptrace` attach fails if already traced).
- Cannot trace processes you don't own (unless root or have `CAP_SYS_PTRACE`).
- YAMA ptrace scope may restrict attaching to non-child processes:
  ```bash
  cat /proc/sys/kernel/yama/ptrace_scope
  # 0 = all processes, 1 = only child processes, 2 = admin only, 3 = disabled
  ```

---

## 12. The `/proc` Filesystem for Process Inspection

### 12.1 Per-Process `/proc` Entries

Every running process has a directory `/proc/<pid>/` with detailed information.

```bash
# List all entries for a process
ls -la /proc/<pid>/

# Key files for stop/resume understanding:
```

| File | Content |
|---|---|
| `/proc/<pid>/status` | Human-readable process status (state, signals, etc.) |
| `/proc/<pid>/stat` | Machine-readable process stats |
| `/proc/<pid>/state` | Process state (WCHAN info, kernel 4.14+) |
| `/proc/<pid>/cmdline` | Command line arguments (null-separated) |
| `/proc/<pid>/stack` | Kernel stack trace (requires root) |
| `/proc/<pid>/wchan` | Kernel function where process is sleeping |
| `/proc/<pid>/syscall` | Current syscall number and arguments |
| `/proc/<pid>/fd/` | Open file descriptors |
| `/proc/<pid>/maps` | Memory mappings |
| `/proc/<pid>/mem` | Process memory (requires ptrace permissions) |
| `/proc/<pid>/environ` | Environment variables |
| `/proc/<pid>/limits` | Resource limits |
| `/proc/<pid>/cgroup` | Cgroup membership |
| `/proc/<pid>/ns/` | Namespace file descriptors |
| `/proc/<pid>/task/` | Thread information |
| `/proc/<pid>/oom_score` | OOM killer score |
| `/proc/<pid>/oom_score_adj` | OOM score adjustment |

### 12.2 Reading Process State from `/proc`

```bash
# Human-readable status
cat /proc/<pid>/status

# Key fields:
# Name:    bash
# State:   S (sleeping)
# Tgid:    1234
# Ngid:    0
# Pid:     1234
# PPid:    1233
# TracerPid: 0        ← 0 if not being traced, PID of tracer if being debugged
# Uid:     1000  1000  1000  1000
# Gid:     1000  1000  1000  1000
# FDSize:  256
# Threads: 1
# SigQ:    0/15566
# SigPnd:  0000000000000000    ← Pending signals
# ShdPnd:  0000000000000000    ← Shared pending signals (thread group)
# SigBlk:  0000000000000000    ← Blocked signals
# SigIgn:  0000000000000000    ← Ignored signals
# SigCgt:  0000000000000000    ← Caught signals
# CapInh:  0000000000000000
# CapPrm:  0000000000000000
# CapEff:  0000000000000000
# CapBnd:  0000000000000000
# voluntary_ctxt_switches:  542
# nonvoluntary_ctxt_switches: 12
```

### 12.3 Decoding Signal Masks

```bash
# The signal masks are hexadecimal bitmasks
# Signal number N corresponds to bit (N-1)

python3 << 'EOF'
def decode_signal_mask(hex_mask):
    mask = int(hex_mask, 16)
    signals = {
        1: "SIGHUP", 2: "SIGINT", 3: "SIGQUIT", 4: "SIGILL",
        5: "SIGTRAP", 6: "SIGABRT", 7: "SIGBUS", 8: "SIGFPE",
        9: "SIGKILL", 10: "SIGUSR1", 11: "SIGSEGV", 12: "SIGUSR2",
        13: "SIGPIPE", 14: "SIGALRM", 15: "SIGTERM", 17: "SIGCHLD",
        18: "SIGCONT", 19: "SIGSTOP", 20: "SIGTSTP", 23: "SIGURG",
        24: "SIGXCPU", 25: "SIGXFSZ", 26: "SIGVTALRM", 27: "SIGPROF",
        28: "SIGWINCH", 29: "SIGIO", 30: "SIGPWR", 31: "SIGSYS"
    }

    print(f"Hex mask: {hex_mask}")
    print(f"Binary:   {bin(mask)}")
    print("Active signals:")
    for i in range(1, 32):
        if mask & (1 << (i-1)):
            name = signals.get(i, f"SIG{i}")
            print(f"  {i:2d} ({name})")

# Example: decode from /proc/<pid>/status
import sys
if len(sys.argv) > 1:
    decode_signal_mask(sys.argv[1])
else:
    decode_signal_mask("0000000000001004")
EOF
```

### 12.4 Checking What a Process Is Waiting For

```bash
# What kernel function is the process waiting in?
cat /proc/<pid>/wchan
# "do_epoll_wait" — waiting in epoll
# "unix_stream_read_generic" — waiting on socket read
# "0" — running or not sleeping

# Kernel stack trace (requires root or appropriate capabilities)
cat /proc/<pid>/stack
# Shows the full call stack of where the process is blocked

# Current syscall
cat /proc/<pid>/syscall
# Format: <syscall_number> <arg0> <arg1> ...
# -1 means not in a syscall
```

---

## 13. C Programming: Controlling Processes Programmatically

### 13.1 Sending Signals from C

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>

// Send SIGSTOP to a process
int stop_process(pid_t pid) {
    return kill(pid, SIGSTOP);
}

// Send SIGCONT to a process
int resume_process(pid_t pid) {
    return kill(pid, SIGCONT);
}

// Send SIGTSTP to a process
int suspend_process(pid_t pid) {
    return kill(pid, SIGTSTP);
}

// Send SIGTERM to a process
int terminate_process(pid_t pid) {
    return kill(pid, SIGTERM);
}

// Toggle stop/resume
int toggle_process(pid_t pid) {
    char path[256];
    snprintf(path, sizeof(path), "/proc/%d/status", pid);

    FILE *f = fopen(path, "r");
    if (!f) {
        perror("fopen");
        return -1;
    }

    char line[256];
    char state = '\0';
    while (fgets(line, sizeof(line), f)) {
        if (sscanf(line, "State: %c", &state) == 1) {
            break;
        }
    }
    fclose(f);

    if (state == 'T' || state == 't') {
        printf("Process %d is stopped, resuming...\n", pid);
        return kill(pid, SIGCONT);
    } else {
        printf("Process %d is running, stopping...\n", pid);
        return kill(pid, SIGSTOP);
    }
}

int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <stop|resume|toggle> <pid>\n", argv[0]);
        return 1;
    }

    pid_t pid = atoi(argv[2]);
    int result;

    if (strcmp(argv[1], "stop") == 0) {
        result = stop_process(pid);
    } else if (strcmp(argv[1], "resume") == 0) {
        result = resume_process(pid);
    } else if (strcmp(argv[1], "toggle") == 0) {
        result = toggle_process(pid);
    } else {
        fprintf(stderr, "Unknown action: %s\n", argv[1]);
        return 1;
    }

    if (result == -1) {
        perror("kill");
        return 1;
    }

    printf("Signal sent successfully\n");
    return 0;
}
```

### 13.2 Creating a Process That Handles SIGSTOP/SIGCONT

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <time.h>

volatile sig_atomic_t running = 1;

void handle_cont(int sig) {
    printf("[%ld] SIGCONT received — resuming work\n", time(NULL));
}

void handle_tstp(int sig) {
    printf("[%ld] SIGTSTP received — will be stopped\n", time(NULL));
}

int main() {
    struct sigaction sa;

    sa.sa_handler = handle_cont;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;
    sigaction(SIGCONT, &sa, NULL);

    sa.sa_handler = handle_tstp;
    sigaction(SIGTSTP, &sa, NULL);

    printf("PID: %d\n", getpid());
    printf("Send SIGSTOP to stop me: kill -STOP %d\n", getpid());
    printf("Send SIGCONT to resume:  kill -CONT %d\n", getpid());
    printf("Send SIGTERM to end:     kill %d\n", getpid());

    int counter = 0;
    while (running) {
        counter++;
        printf("[%ld] Working... iteration %d\n", time(NULL), counter);
        sleep(2);
    }

    printf("Exiting gracefully\n");
    return 0;
}
```

### 13.3 Waiting for Child State Changes

```c
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child
        printf("Child PID: %d\n", getpid());
        while (1) {
            printf("Child working...\n");
            sleep(1);
        }
    } else {
        // Parent
        printf("Parent: Child PID is %d\n", pid);
        sleep(2);

        // Stop the child
        printf("Parent: Stopping child\n");
        kill(pid, SIGSTOP);

        // Wait for child to stop
        int status;
        waitpid(pid, &status, WUNTRACED);

        if (WIFSTOPPED(status)) {
            printf("Child stopped with signal %d\n", WSTOPSIG(status));
        }

        sleep(3);

        // Resume the child
        printf("Parent: Resuming child\n");
        kill(pid, SIGCONT);

        sleep(2);

        // Stop again and check
        kill(pid, SIGSTOP);
        waitpid(pid, &status, WUNTRACED);

        if (WIFSTOPPED(status)) {
            printf("Child stopped again with signal %d\n", WSTOPSIG(status));
        }

        // Finally, terminate the child
        printf("Parent: Terminating child\n");
        kill(pid, SIGTERM);
        waitpid(pid, &status, 0);

        if (WIFEXITED(status)) {
            printf("Child exited with status %d\n", WEXITSTATUS(status));
        }
    }

    return 0;
}
```

---

## 14. Python: Controlling Processes Programmatically

### 14.1 Using `os` Module

```python
import os
import signal
import subprocess
import time

# Start a background process
proc = subprocess.Popen(["sleep", "300"])
print(f"Started process with PID: {proc.pid}")

# Stop the process
os.kill(proc.pid, signal.SIGSTOP)
print(f"Process {proc.pid} stopped")

# Check process state
import pathlib
status = pathlib.Path(f"/proc/{proc.pid}/status").read_text()
for line in status.splitlines():
    if line.startswith("State:"):
        print(f"State: {line}")

time.sleep(2)

# Resume the process
os.kill(proc.pid, signal.SIGCONT)
print(f"Process {proc.pid} resumed")

# Stop all child processes of a parent
def stop_process_tree(ppid):
    """Stop all descendants of a process."""
    import pathlib
    children = []
    for pid_dir in pathlib.Path("/proc").glob("[0-9]*"):
        try:
            pid = int(pid_dir.name)
            stat = pathlib.Path(f"/proc/{pid}/stat").read_text()
            fields = stat.split()
            if int(fields[3]) == ppid:  # PPid is field 4 (0-indexed: 3)
                children.append(pid)
        except (FileNotFoundError, PermissionError, ValueError):
            pass

    for pid in children:
        try:
            os.kill(pid, signal.SIGSTOP)
            print(f"Stopped child PID: {pid}")
        except ProcessLookupError:
            pass
    return children

# Resume all child processes
def resume_process_tree(ppid):
    """Resume all descendants of a process."""
    # Same logic as above, but SIGCONT
    pass

# Toggle stop/resume
def toggle_process(pid):
    """Toggle a process between stopped and running."""
    try:
        status = pathlib.Path(f"/proc/{pid}/status").read_text()
        for line in status.splitlines():
            if line.startswith("State:"):
                state = line.split()[1]
                if state in ('T', 't'):
                    os.kill(pid, signal.SIGCONT)
                    return "resumed"
                else:
                    os.kill(pid, signal.SIGSTOP)
                    return "stopped"
    except (FileNotFoundError, PermissionError):
        return "error"

# Send signal to process group
os.killpg(os.getpgid(proc.pid), signal.SIGSTOP)

# Wait for process with timeout
try:
    proc.wait(timeout=5)
except subprocess.TimeoutExpired:
    print("Process didn't finish in 5 seconds")
    proc.kill()  # SIGKILL
```

### 14.2 Using `psutil` Library

```python
import psutil

# Find a process by name
for proc in psutil.process_iter(['pid', 'name', 'status']):
    if 'python' in proc.info['name'].lower():
        print(f"Found: PID={proc.info['pid']}, Status={proc.info['status']}")

# Stop a process
proc = psutil.Process(1234)
proc.suspend()   # Sends SIGSTOP
print(f"Status after suspend: {proc.status()}")  # 'stopped'

# Resume a process
proc.resume()    # Sends SIGCONT
print(f"Status after resume: {proc.status()}")   # 'running' or 'sleeping'

# Get detailed info
print(f"Name: {proc.name()}")
print(f"Status: {proc.status()}")
print(f"PPID: {proc.ppid()}")
print(f"Children: {[p.pid for p in proc.children()]}")
print(f"Num threads: {proc.num_threads()}")

# Stop entire process tree
def stop_tree(pid):
    try:
        parent = psutil.Process(pid)
        children = parent.children(recursive=True)
        children.append(parent)
        for child in children:
            try:
                child.suspend()
                print(f"Suspended PID {child.pid}")
            except (psutil.NoSuchProcess, psutil.AccessDenied) as e:
                print(f"Error with PID {child.pid}: {e}")
    except psutil.NoSuchProcess:
        print(f"Process {pid} not found")

# Resume entire process tree
def resume_tree(pid):
    try:
        parent = psutil.Process(pid)
        children = parent.children(recursive=True)
        children.append(parent)
        # Resume children first, then parent
        for child in reversed(children):
            try:
                child.resume()
                print(f"Resumed PID {child.pid}")
            except (psutil.NoSuchProcess, psutil.AccessDenied) as e:
                print(f"Error with PID {child.pid}: {e}")
    except psutil.NoSuchProcess:
        print(f"Process {pid} not found")

# Monitor process state changes
import time

proc = psutil.Process(1234)
prev_status = proc.status()

while True:
    try:
        current = proc.status()
        if current != prev_status:
            print(f"State changed: {prev_status} -> {current}")
            prev_status = current
    except psutil.NoSuchProcess:
        print("Process terminated")
        break
    time.sleep(0.5)
```

### 14.3 Using `subprocess` for Job Control

```python
import subprocess
import os
import signal
import time

# Start a process
proc = subprocess.Popen(
    ["python3", "-c", "import time; [print(i) or time.sleep(1) for i in range(100)]"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE
)

# Let it run
time.sleep(3)

# Stop it
proc.send_signal(signal.SIGSTOP)
print(f"Process stopped, status: {proc.poll()}")  # None = still alive

time.sleep(2)

# Resume it
proc.send_signal(signal.SIGCONT)
print(f"Process resumed")

# Wait for completion
proc.wait()
print(f"Process exited with code: {proc.returncode}")
```

---

## 15. Stopping Processes via cgroups

### 15.1 cgroup v1 — CPU CFS Quota Method

You can effectively "stop" a process by setting its CPU quota to 0.

```bash
# Create a cgroup
mkdir -p /sys/fs/cgroup/cpu/stopped_group

# Move a process into the cgroup
echo <pid> > /sys/fs/cgroup/cpu/stopped_group/cgroup.procs

# Set CPU quota to 0 (process gets no CPU time)
echo 1 > /sys/fs/cgroup/cpu/stopped_group/cpu.cfs_period_us
echo 0 > /sys/fs/cgroup/cpu/stopped_group/cpu.cfs_quota_us

# To resume, restore the quota
echo 100000 > /sys/fs/cgroup/cpu/stopped_group/cpu.cfs_quota_us
```

### 15.2 cgroup v2 — CPU Max Method

```bash
# Enable cgroup v2 hierarchy
# (requires kernel 5.2+, mounted at /sys/fs/cgroup)

# Create a cgroup
mkdir /sys/fs/cgroup/stopped

# Move process
echo <pid> > /sys/fs/cgroup/stopped/cgroup.procs

# Set CPU max to 0 (effectively stops the process)
echo "1 100000" > /sys/fs/cgroup/stopped/cpu.max

# Resume
echo "max 100000" > /sys/fs/cgroup/stopped/cpu.max
```

### 15.3 systemd Integration

```bash
# Stop a service (systemd sends SIGTERM, then SIGKILL after timeout)
systemctl stop <service>

# Pause a service's cgroup (systemd 244+)
systemctl stop --job-mode=replace <service>

# Freeze a service (sends SIGSTOP to all processes)
systemctl freeze <service>

# Thaw a service (sends SIGCONT to all processes)
systemctl thaw <service>

# Check freeze state
systemctl show <service> -p SubState

# Set CPU quota via systemd
systemctl set-property <service> CPUQuota=0%
systemctl set-property <service> CPUQuota=100%
```

---

## 16. Freezing Processes via the CGroup Freezer

### 16.1 What Is the CGroup Freezer?

The cgroup freezer subsystem allows you to **atomically freeze and thaw** an entire group of processes. Unlike `SIGSTOP` (which stops individual processes), the freezer stops **all processes in the cgroup simultaneously**.

Frozen processes:
- Are completely removed from the scheduler.
- Consume zero CPU time.
- Cannot receive or process signals (signals are queued until thaw).
- Are ideal for checkpointing, live migration, and system maintenance.

### 16.2 cgroup v1 Freezer

```bash
# Create a freezer cgroup
mkdir -p /sys/fs/cgroup/freezer/myfrozen

# Move processes into the cgroup
echo <pid1> > /sys/fs/cgroup/freezer/myfrozen/cgroup.procs
echo <pid2> >> /sys/fs/cgroup/freezer/myfrozen/cgroup.procs

# Freeze the group
echo FROZEN > /sys/fs/cgroup/freezer/myfrozen/freezer.state

# Check state
cat /sys/fs/cgroup/freezer/myfrozen/freezer.state
# FREEZING → transitioning
# FROZEN → all processes stopped

# Thaw the group
echo THAWED > /sys/fs/cgroup/freezer/myfrozen/freezer.state

# Verify
cat /sys/fs/cgroup/freezer/myfrozen/freezer.state
# THAWED

# Clean up
rmdir /sys/fs/cgroup/freezer/myfrozen
```

### 16.3 cgroup v2 Freezer

In cgroup v2, the freezer is integrated into the core cgroup controller.

```bash
# Create cgroup
mkdir /sys/fs/cgroup/myfrozen

# Move processes
echo <pid> > /sys/fs/cgroup/myfrozen/cgroup.procs

# Freeze (write '1' to freeze, '0' to thaw)
echo 1 > /sys/fs/cgroup/myfrozen/cgroup.freeze

# Check freeze state
cat /sys/fs/cgroup/myfrozen/cgroup.freeze
# 1 = frozen, 0 = thawed

# Thaw
echo 0 > /sys/fs/cgroup/myfrozen/cgroup.freeze

# Recursive freeze (freeze all descendants too)
echo 1 > /sys/fs/cgroup/cgroup.freeze
```

### 16.4 Using `systemd` to Freeze Services

```bash
# Freeze a systemd service
systemctl freeze nginx.service

# Check status (shows "frozen")
systemctl status nginx.service

# Thaw the service
systemctl thaw nginx.service

# Freeze multiple services
systemctl freeze nginx.service php-fpm.service mysql.service

# Thaw all
systemctl thaw nginx.service php-fpm.service mysql.service
```

### 16.5 Freezer vs SIGSTOP

| Feature | SIGSTOP | CGroup Freezer |
|---|---|---|
| Scope | Single process | Entire cgroup (atomic) |
| Signal handling | Signals delivered immediately | Signals queued until thaw |
| CPU consumption | Process still scheduled (but stops) | Completely removed from scheduler |
| Checkpointing | Not safe | Safe for checkpointing |
| Granularity | Per-process | Per-cgroup |
| API | `kill()` syscall | cgroup filesystem |
| Nesting | N/A | Supports nested cgroups |
| State visibility | `ps` shows 'T' | cgroup file shows FROZEN |

---

## 17. Checkpoint and Restore (CRIU)

### 17.1 What Is CRIU?

**CRIU** (Checkpoint/Restore In Userspace) is a Linux tool that can **freeze a running application** and save its complete state (memory, open files, network connections, etc.) to disk. The application can later be **restored** to exactly the same state, potentially on a different machine.

### 17.2 Installation

```bash
# Debian/Ubuntu
apt install criu

# RHEL/CentOS/Fedora
dnf install criu

# Arch Linux
pacman -S criu
```

### 17.3 Basic Usage

```bash
# Check if CRIU works on your system
criu check

# Dump (checkpoint) a process
criu dump -t <pid> -D /path/to/checkpoint-dir --shell-job

# Restore a process
criu restore -D /path/to/checkpoint-dir --shell-job

# Dump with specific options
criu dump -t <pid> \
    -D /checkpoint/ \
    --shell-job \
    --ext-unix-sk \
    --tcp-established \
    -v4  # verbose level 4

# Restore on the same machine
criu restore -D /checkpoint/ --shell-job

# Restore on a different machine (requires matching environment)
criu restore -D /checkpoint/ --shell-job --restore-detached
```

### 17.4 CRIU Options

```bash
# Leave process running after dump (pre-copy migration)
criu dump -t <pid> -D /checkpoint/ --leave-running

# Dump specific file descriptors
criu dump -t <pid> -D /checkpoint/ --external file:/path/to/file

# Handle TCP connections
criu dump -t <pid> -D /checkpoint/ --tcp-established

# Handle Unix sockets
criu dump -t <pid> -D /checkpoint/ --ext-unix-sk

# Handle pipes
criu dump -t <pid> -D /checkpoint/ --handle-file-mode

# Pre-dump (for live migration, process keeps running)
criu pre-dump -t <pid> -D /checkpoint/ -o /checkpoint/pre-dump.log

# Iterative checkpoint
criu dump -t <pid> -D /checkpoint/ --prev-images-dir /checkpoint/

# Restore in background
criu restore -D /checkpoint/ --restore-detached
```

### 17.5 Practical CRIU Workflow

```bash
# 1. Start a process
python3 -c "
import time
counter = 0
while True:
    counter += 1
    print(f'Counter: {counter}')
    time.sleep(1)
" &
PID=$!
echo "PID: $PID"

# 2. Checkpoint it
mkdir -p /tmp/checkpoint
criu dump -t $PID -D /tmp/checkpoint --shell-job -v4

# 3. Verify it's gone
ps -p $PID   # Should not exist

# 4. Restore it
criu restore -D /tmp/checkpoint --shell-job

# 5. Verify it's back (with same counter value)
ps aux | grep python
```

### 17.6 CRIU Limitations

- Requires kernel support (CONFIG_CHECKPOINT_RESTORE).
- Cannot checkpoint kernel threads.
- Some kernel features are not supported (certain device drivers).
- Network sockets may need `--tcp-established`.
- Requires matching library versions for restore.
- Processes in D state cannot be checkpointed.

---

## 18. Handling Unstoppable Processes (D State)

### 18.1 What Is D State?

D state (TASK_UNINTERRUPTIBLE) means a process is waiting for I/O and **cannot be interrupted by any signal**. This includes `SIGKILL` and `SIGSTOP`.

### 18.2 Why Can't D State Processes Be Stopped?

The kernel marks certain I/O operations as uninterruptible to protect kernel data structures from corruption. If the process could be killed while holding kernel locks, the filesystem or driver could be left in an inconsistent state.

### 18.3 Identifying D State Processes

```bash
# Find all D state processes
ps -eo pid,ppid,stat,wchan:30,comm | awk '$3 ~ /^D/'

# Show what they're waiting for
for pid in $(ps -eo pid --no-headers | awk '{print $1}'); do
    state=$(ps -o stat= -p $pid 2>/dev/null)
    if [[ $state == D* ]]; then
        echo "PID $pid is in D state"
        echo "  Command: $(ps -o comm= -p $pid)"
        echo "  WCHAN: $(cat /proc/$pid/wchan)"
        echo "  Stack:"
        cat /proc/$pid/stack 2>/dev/null | sed 's/^/    /'
        echo ""
    fi
done

# Quick one-liner
ps -eo pid,stat,wchan:30,comm | grep -E '^\s*[0-9]+\s+D'
```

### 18.4 Common Causes of D State

| Cause | WCHAN shows | Solution |
|---|---|---|
| NFS server unreachable | `nfs_wait_bit_uninterruptible`, `rpc_wait_bit_killable` | Fix NFS server, use `umount -f -l` |
| Disk I/O hang | `io_schedule`, `blk_queue_enter`, `__d_wait` | Check disk health, replace failing hardware |
| DLM lock contention | `dlm_lock` | Fix cluster lock issues |
| Slow USB device | `usb_storage` | Disconnect/reconnect device |
| FUSE filesystem hang | `fuse_wait_request` | Restart FUSE daemon |
| LVM/DM hang | `dm_io`, `dm_request` | Check device mapper status |

### 18.5 Resolving D State Processes

```bash
# 1. Identify the blocked I/O
cat /proc/<pid>/stack
cat /proc/<pid>/wchan

# 2. Check dmesg for I/O errors
dmesg | tail -50
dmesg | grep -i error
dmesg | grep -i timeout

# 3. Check storage devices
lsblk
iostat -x 1
smartctl -a /dev/sda

# 4. For NFS issues
showmount -e <nfs_server>
nfsstat -c

# 5. Force unmount stuck NFS (last resort)
umount -f -l /mnt/nfs

# 6. Reset SCSI bus
echo 1 > /sys/block/sda/device/rescan

# 7. If hardware issue, may need reboot
# There is NO way to kill a D state process without reboot
```

### 18.6 Preventing D State Issues

```bash
# Use killable sleep for NFS mounts (kernel 2.6.25+)
# Modern kernels use TASK_KILLABLE for most NFS operations

# Mount NFS with interruptible option
mount -t nfs -o intr <server>:/share /mnt/nfs

# Set I/O timeouts
# For iSCSI:
iscsiadm -m node -T <target> -o update -n node.session.timeo.replacement_timeout -v 30

# Use autofs for on-demand mounts
# Use systemd mount units with TimeoutIdleSec
```

---

## 19. Practical Scenarios and Recipes

### 19.1 Stop a Runaway Process for Investigation

```bash
# 1. Find the process
top
# or
ps aux --sort=-%cpu | head

# 2. Stop it immediately
kill -STOP <pid>

# 3. Investigate while it's frozen
cat /proc/<pid>/stack        # Kernel stack
cat /proc/<pid>/status       # Process status
cat /proc/<pid>/maps         # Memory map
cat /proc/<pid>/fd/          # Open files
strace -p <pid> -e trace=none  # Just attach
ls -l /proc/<pid>/fd/        # Check open files
lsof -p <pid>                # List open files

# 4. Take a core dump for analysis
gcore <pid>                  # Creates core.<pid>

# 5. Resume it
kill -CONT <pid>
```

### 19.2 Temporarily Pause All User Processes

```bash
# Stop all processes owned by a user (except shell)
pkill -STOP -u $USER

# Resume
pkill -CONT -u $USER

# Stop all except specific processes
pkill -STOP -v -u root    # Stop everything except root's processes

# Stop all non-kernel processes
for pid in $(ps -eo pid --no-headers); do
    state=$(ps -o stat= -p $pid 2>/dev/null)
    if [[ -n $state && ! $state == *"("* ]]; then
        kill -STOP $pid 2>/dev/null
    fi
done

# Resume all
for pid in $(ps -eo pid --no-headers); do
    kill -CONT $pid 2>/dev/null
done
```

### 19.3 Freeze a Service During Maintenance

```bash
# Using systemd freeze
systemctl freeze nginx.service

# Do maintenance work
# ... backup, config changes, etc. ...

# Thaw the service
systemctl thaw nginx.service

# Alternative: manual SIGSTOP/SIGCONT for all processes
pgrep -f nginx | xargs kill -STOP
# ... maintenance ...
pgrep -f nginx | xargs kill -CONT
```

### 19.4 Pause Resource-Heavy Background Job

```bash
# Find CPU-heavy processes
ps aux --sort=-%cpu | head -5

# Pause the top one
top_cpu_pid=$(ps -eo pid,%cpu --sort=-%cpu --no-headers | head -1 | awk '{print $1}')
kill -STOP $top_cpu_pid

# Check system is happier now
top

# Resume when ready
kill -CONT $top_cpu_pid

# Or use nice/ionice for lower priority instead of full stop
renice +19 -p $top_cpu_pid
ionice -c 3 -p $top_cpu_pid    # Idle I/O class
```

### 19.5 Debug a Process Without Restarting

```bash
# 1. Stop the process
kill -STOP <pid>

# 2. Attach gdb
gdb -p <pid>

# Inside gdb:
(gdb) info threads
(gdb) thread apply all bt
(gdb) frame 3
(gdb) info locals
(gdb) print some_variable

# 3. Set a breakpoint for future occurrence
(gdb) break problematic_function

# 4. Continue
(gdb) continue

# 5. Detach when done
(gdb) detach
(gdb) quit
```

### 19.6 Stop All Processes Before Taking a Snapshot

```bash
#!/bin/bash
# Freeze all user-space processes for consistent snapshot

# Get all non-kernel process PIDs
PIDS=$(ps -eo pid --no-headers | awk '{print $1}')

echo "Freezing all processes..."
for pid in $PIDS; do
    # Skip kernel threads (PPID 2 = kthreadd)
    ppid=$(ps -o ppid= -p $pid 2>/dev/null)
    if [ "$ppid" != "     2" ] && [ "$pid" != "1" ]; then
        kill -STOP $pid 2>/dev/null
    fi
done

echo "All processes frozen. Taking snapshot..."
# Take your snapshot here
lvcreate --snapshot -n snap -L 10G /dev/vg0/lv_root

echo "Thawing all processes..."
for pid in $PIDS; do
    kill -CONT $pid 2>/dev/null
done

echo "Done."
```

### 19.7 Create a Custom "Pause/Resume" Wrapper

```bash
#!/bin/bash
# pr — pause/resume processes by name or PID
# Usage: pr <stop|resume|toggle> <pid|name>

action=$1
target=$2

if [[ -z "$action" || -z "$target" ]]; then
    echo "Usage: $0 <stop|resume|toggle> <pid|name>"
    exit 1
fi

# Determine if target is PID or name
if [[ "$target" =~ ^[0-9]+$ ]]; then
    # It's a PID
    pids="$target"
else
    # It's a name — find matching PIDs
    pids=$(pgrep -f "$target")
fi

case "$action" in
    stop|pause)
        signal=SIGSTOP
        action_text="Stopping"
        ;;
    resume|cont)
        signal=SIGCONT
        action_text="Resuming"
        ;;
    toggle)
        # Check state and decide
        for pid in $pids; do
            state=$(ps -o stat= -p $pid 2>/dev/null)
            if [[ $state == *T* ]]; then
                kill -CONT $pid && echo "Resumed PID $pid"
            else
                kill -STOP $pid && echo "Stopped PID $pid"
            fi
        done
        exit
        ;;
    *)
        echo "Unknown action: $action"
        exit 1
        ;;
esac

for pid in $pids; do
    if kill -s "$signal" "$pid" 2>/dev/null; then
        echo "$action_text PID $pid"
    else
        echo "Failed to signal PID $pid"
    fi
done
```

### 19.8 Monitor and Auto-Pause High CPU Usage

```bash
#!/bin/bash
# Auto-pause processes that exceed CPU threshold

THRESHOLD=90  # CPU percent
CHECK_INTERVAL=5

while true; do
    while IFS= read -r line; do
        pid=$(echo "$line" | awk '{print $1}')
        cpu=$(echo "$line" | awk '{print $2}')
        comm=$(echo "$line" | awk '{print $3}')

        # Compare CPU usage (integer comparison)
        cpu_int=${cpu%.*}
        if (( cpu_int > THRESHOLD )); then
            echo "[$(date)] $comm (PID $pid) at ${cpu}% — pausing"
            kill -STOP "$pid"
            echo "[$(date)] Paused. Waiting 30 seconds before resume..."
            sleep 30
            kill -CONT "$pid"
            echo "[$(date)] Resumed $comm"
        fi
    done < <(ps -eo pid,%cpu,comm --no-headers --sort=-%cpu | head -5)

    sleep $CHECK_INTERVAL
done
```

### 19.9 Stop Processes Matching Complex Criteria

```bash
# Stop all Python processes running a specific script
pkill -STOP -f "python3 /opt/app/worker.py"

# Stop all processes in a specific directory
for pid in $(pgrep -f "/opt/app/"); do
    kill -STOP $pid
done

# Stop all processes opened by a specific user on a specific TTY
pkill -STOP -u john -t pts/0

# Stop all child and grandchild processes of a parent
pstree -p <pid> | grep -oP '\d+' | xargs -I{} kill -STOP {}

# Stop all threads of a multi-threaded process
for tid in $(ls /proc/<pid>/task/); do
    kill -STOP $tid
done

# Resume threads
for tid in $(ls /proc/<pid>/task/); do
    kill -CONT $tid
done
```

### 19.10 Forensic Process Freeze

```bash
#!/bin/bash
# Freeze a suspicious process and capture its state for analysis

PID=$1
if [[ -z "$PID" ]]; then
    echo "Usage: $0 <pid>"
    exit 1
fi

SNAPSHOT_DIR="/tmp/forensic_${PID}_$(date +%s)"
mkdir -p "$SNAPSHOT_DIR"

echo "=== Freezing process $PID ==="
kill -STOP $PID
sleep 1

echo "=== Capturing process state ==="

# Basic info
ps -fp $PID > "$SNAPSHOT_DIR/ps.txt"
cat /proc/$PID/status > "$SNAPSHOT_DIR/status.txt"
cat /proc/$PID/stat > "$SNAPSHOT_DIR/stat.txt"
cat /proc/$PID/cmdline | tr '\0' ' ' > "$SNAPSHOT_DIR/cmdline.txt"

# Memory
cat /proc/$PID/maps > "$SNAPSHOT_DIR/maps.txt"
cat /proc/$PID/smaps > "$SNAPSHOT_DIR/smaps.txt"
cat /proc/$PID/numa_maps > "$SNAPSHOT_DIR/numa_maps.txt"

# File descriptors
ls -l /proc/$PID/fd/ > "$SNAPSHOT_DIR/fd_list.txt"
cat /proc/$PID/fdinfo/* > "$SNAPSHOT_DIR/fdinfo.txt" 2>/dev/null

# Network connections
cat /proc/$PID/net/tcp > "$SNAPSHOT_DIR/net_tcp.txt" 2>/dev/null
cat /proc/$PID/net/udp > "$SNAPSHOT_DIR/net_udp.txt" 2>/dev/null

# Environment
cat /proc/$PID/environ | tr '\0' '\n' > "$SNAPSHOT_DIR/environ.txt"

# Stack trace
cat /proc/$PID/stack > "$SNAPSHOT_DIR/stack.txt" 2>/dev/null

# Core dump
gcore -o "$SNAPSHOT_DIR/core" $PID 2>/dev/null

# Memory dump (requires root)
# cat /proc/$PID/mem > "$SNAPSHOT_DIR/mem.raw" 2>/dev/null

echo "=== Resuming process ==="
kill -CONT $PID

echo "=== Snapshot saved to $SNAPSHOT_DIR ==="
ls -la "$SNAPSHOT_DIR"
```

---

## 20. Troubleshooting Common Issues

### 20.1 Process Won't Stop with SIGSTOP

**Problem:** `kill -STOP <pid>` seems to have no effect.

**Causes and solutions:**

1. **Process is in D state (uninterruptible sleep):**
   ```bash
   ps -o stat= -p <pid>
   # If starts with D, you cannot stop it
   # Fix the underlying I/O issue
   ```

2. **Process is a zombie:**
   ```bash
   ps -o stat= -p <pid>
   # If Z, process is already dead — can't stop it
   # Kill the parent to reap it
   ```

3. **You lack permissions:**
   ```bash
   # Can't stop processes owned by other users (unless root)
   sudo kill -STOP <pid>
   ```

4. **Process is PID 0 or PID 1:**
   ```bash
   # PID 0 (idle task) cannot be signaled
   # PID 1 (init) ignores SIGSTOP
   ```

### 20.2 Process Won't Resume with SIGCONT

**Problem:** `kill -CONT <pid>` doesn't resume the process.

**Causes and solutions:**

1. **Process is in D state:**
   ```bash
   # Cannot resume a D state process with SIGCONT
   # Fix the I/O issue
   ```

2. **Process was stopped by ptrace (gdb/strace):**
   ```bash
   # If tracer is still attached, SIGCONT won't fully resume
   # Detach the tracer first
   # In gdb: (gdb) detach
   # Or: strace is intercepting — stop strace
   ```

3. **Process is in T state but not from SIGSTOP:**
   ```bash
   # Check what stopped it
   cat /proc/<pid>/status | grep State
   # If stopped by SIGTTIN or SIGTTOU, it needs terminal access
   ```

### 20.3 Background Process Gets Stopped Unexpectedly

**Problem:** A background job stops with message "Stopped (tty output)" or "Stopped (tty input)".

**Cause:** The process tried to read from (`SIGTTIN`) or write to (`SIGTTOU`) the terminal while in the background.

**Solutions:**
```bash
# Resume in background
bg %1

# Or redirect I/O to prevent terminal access
command > /dev/null 2>&1 < /dev/null &

# Or use nohup
nohup command > output.log 2>&1 &

# Or use setsid
setsid command &

# Or redirect stdin to prevent SIGTTIN
command < /dev/null &
```

### 20.4 Terminal Freezes After Process Stops

**Problem:** Terminal becomes unresponsive after stopping/resuming processes.

**Cause:** The terminal's process group state got confused, or XON/XOFF flow control was triggered.

**Solutions:**
```bash
# Reset the terminal
reset
# or
stty sane

# If flow control was triggered (Ctrl+S stopped output)
# Press Ctrl+Q to resume

# Kill the stopped job if terminal is stuck
kill %1
```

### 20.5 "Operation not permitted" When Sending Signals

**Problem:** `kill -STOP <pid>` returns "Operation not permitted."

**Causes:**
```bash
# 1. Process owned by another user
sudo kill -STOP <pid>

# 2. YAMA ptrace scope restriction
cat /proc/sys/kernel/yama/ptrace_scope
# If 1 or higher, you can only signal child processes
# Temporarily change:
echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope

# 3. Docker/container restrictions
# Container may not have CAP_KILL
# Run container with --cap-add=SYS_PTRACE

# 4. SELinux/AppArmor blocking
# Check audit log
sudo ausearch -m avc -ts recent
```

### 20.6 Signals Being Ignored by Process

**Problem:** `kill -TERM <pid>` has no effect, but `kill -KILL <pid>` works.

**Cause:** The process has registered a signal handler that ignores SIGTERM, or has set it to SIG_IGN.

**Diagnosis:**
```bash
cat /proc/<pid>/status | grep SigIgn
# Check if SIGTERM (bit 14) is set

# Decode the mask
python3 -c "
mask = 0x$(cat /proc/<pid>/status | grep SigIgn | awk '{print $2}')
print('SIGTERM ignored' if mask & (1 << 14) else 'SIGTERM not ignored')
"
```

**Solutions:**
```bash
# Use SIGKILL (cannot be ignored)
kill -9 <pid>

# Or use SIGQUIT
kill -QUIT <pid>

# Or check if process has a different reload mechanism
kill -HUP <pid>   # Many daemons reload config on HUP
```

### 20.7 Zombie Accumulation

**Problem:** Many zombie processes accumulating.

**Cause:** Parent process is not calling `wait()` on terminated children.

**Diagnosis:**
```bash
ps -eo pid,ppid,stat,comm | awk '$3 ~ /^Z/'

# Count zombies
ps -eo stat | grep -c Z
```

**Resolution:**
```bash
# 1. Send SIGCHLD to parent to prompt reaping
kill -SIGCHLD <parent_pid>

# 2. If parent is buggy, kill the parent
kill <parent_pid>
# Zombies will be reparented to PID 1 and reaped

# 3. Fix the parent's code:
# Add wait()/waitpid() or set SA_NOCLDWAIT flag
```

---

## 21. Complete Command Reference Table

### Stopping Processes

| Command | Description | Example |
|---|---|---|
| `kill -STOP <pid>` | Stop process (SIGSTOP) | `kill -STOP 1234` |
| `kill -19 <pid>` | Stop by signal number | `kill -19 1234` |
| `kill -SIGSTOP <pid>` | Stop by full name | `kill -SIGSTOP 1234` |
| `killall -STOP <name>` | Stop all by name | `killall -STOP nginx` |
| `pkill -STOP <pattern>` | Stop by pattern | `pkill -STOP -f "worker.py"` |
| `kill -STOP -<pgid>` | Stop process group | `kill -STOP -5678` |
| `kill -STOP -u <user>` | Stop user's processes | `kill -STOP -u john` |
| `kill -STOP -t <tty>` | Stop on terminal | `kill -STOP -t pts/0` |
| `kill -STOP %1` | Stop shell job | `kill -STOP %1` |
| `Ctrl+Z` | Stop foreground job | (interactive) |
| `systemctl freeze <svc>` | Freeze systemd service | `systemctl freeze nginx` |
| `echo FROZEN > freezer.state` | Freeze cgroup (v1) | cgroup filesystem |
| `echo 1 > cgroup.freeze` | Freeze cgroup (v2) | cgroup filesystem |
| `os.kill(pid, SIGSTOP)` | Python stop | `import os, signal` |
| `proc.suspend()` | psutil stop | `import psutil` |

### Resuming Processes

| Command | Description | Example |
|---|---|---|
| `kill -CONT <pid>` | Resume process | `kill -CONT 1234` |
| `kill -18 <pid>` | Resume by number | `kill -18 1234` |
| `killall -CONT <name>` | Resume all by name | `killall -CONT nginx` |
| `pkill -CONT <pattern>` | Resume by pattern | `pkill -CONT -f "worker"` |
| `bg %1` | Resume job in background | (shell builtin) |
| `fg %1` | Resume job in foreground | (shell builtin) |
| `systemctl thaw <svc>` | Thaw systemd service | `systemctl thaw nginx` |
| `echo THAWED > freezer.state` | Thaw cgroup (v1) | cgroup filesystem |
| `echo 0 > cgroup.freeze` | Thaw cgroup (v2) | cgroup filesystem |
| `os.kill(pid, SIGCONT)` | Python resume | `import os, signal` |
| `proc.resume()` | psutil resume | `import psutil` |

### Inspection

| Command | Description | Example |
|---|---|---|
| `ps -eo pid,stat,comm` | Show process states | Standard |
| `ps aux \| grep Z` | Find zombies | Quick check |
| `ps -eo pid,stat \| awk '$2~/^T/'` | Find stopped | Filter |
| `jobs -l` | List shell jobs | Shell builtin |
| `cat /proc/<pid>/status` | Detailed status | File read |
| `cat /proc/<pid>/stack` | Kernel stack trace | Requires root |
| `cat /proc/<pid>/wchan` | Wait channel | File read |
| `strace -p <pid>` | Trace syscalls | Debug |
| `gdb -p <pid>` | Attach debugger | Debug |
| `pstree -p <pid>` | Show process tree | Tree view |
| `pgrep -a <name>` | Find by name + cmdline | Search |

### Prevention and Management

| Command | Description | Example |
|---|---|---|
| `nohup cmd &` | Run immune to SIGHUP | Background tasks |
| `disown %1` | Remove from job table | Shell |
| `setsid cmd` | Run in new session | Full detach |
| `renice +19 -p <pid>` | Lower priority | Resource mgmt |
| `ionice -c 3 -p <pid>` | Set idle I/O | Resource mgmt |
| `chrt -i 0 <cmd>` | Idle scheduling | Resource mgmt |
| `ulimit -t <seconds>` | CPU time limit | Prevention |

---

## 22. Signal Reference Table

| Signal | Number | Default | Catchable | Ignorable | Description |
|---|---|---|---|---|---|
| `SIGHUP` | 1 | Term | Yes | Yes | Hangup detected on controlling terminal |
| `SIGINT` | 2 | Term | Yes | Yes | Interrupt from keyboard (Ctrl+C) |
| `SIGQUIT` | 3 | Core | Yes | Yes | Quit from keyboard (Ctrl+\) |
| `SIGILL` | 4 | Core | Yes | No | Illegal instruction |
| `SIGTRAP` | 5 | Core | Yes | No | Trace/breakpoint trap |
| `SIGABRT`/`SIGIOT` | 6 | Core | Yes | No | Abort signal from abort(3) |
| `SIGBUS` | 7 | Core | Yes | No | Bus error (bad memory access) |
| `SIGFPE` | 8 | Core | Yes | No | Floating-point exception |
| `SIGKILL` | 9 | Term | **No** | **No** | Kill signal (guaranteed termination) |
| `SIGUSR1` | 10 | Term | Yes | Yes | User-defined signal 1 |
| `SIGSEGV` | 11 | Core | Yes | No | Invalid memory reference |
| `SIGUSR2` | 12 | Term | Yes | Yes | User-defined signal 2 |
| `SIGPIPE` | 13 | Term | Yes | Yes | Broken pipe |
| `SIGALRM` | 14 | Term | Yes | Yes | Timer signal from alarm(2) |
| `SIGTERM` | 15 | Term | Yes | Yes | Termination signal (default for kill) |
| `SIGCHLD` | 17 | Ign | Yes | Yes | Child stopped or terminated |
| `SIGCONT` | 18 | Cont | Yes | **No** | Continue if stopped |
| `SIGSTOP` | 19 | Stop | **No** | **No** | Stop process |
| `SIGTSTP` | 20 | Stop | Yes | Yes | Terminal stop signal (Ctrl+Z) |
| `SIGTTIN` | 21 | Stop | Yes | Yes | Background read from tty |
| `SIGTTOU` | 22 | Stop | Yes | Yes | Background write to tty |
| `SIGURG` | 23 | Ign | Yes | Yes | Urgent condition on socket |
| `SIGXCPU` | 24 | Core | Yes | Yes | CPU time limit exceeded |
| `SIGXFSZ` | 25 | Core | Yes | Yes | File size limit exceeded |
| `SIGVTALRM` | 26 | Term | Yes | Yes | Virtual alarm clock |
| `SIGPROF` | 27 | Term | Yes | Yes | Profiling timer expired |
| `SIGWINCH` | 28 | Ign | Yes | Yes | Window resize |
| `SIGIO`/`SIGPOLL` | 29 | Term | Yes | Yes | I/O possible |
| `SIGPWR` | 30 | Term | Yes | Yes | Power failure |
| `SIGSYS`/`SIGUNUSED` | 31 | Core | Yes | No | Bad system call |

**Legend:**
- **Term** = Default action is to terminate the process
- **Core** = Terminate and dump core
- **Ign** = Default action is to ignore
- **Stop** = Default action is to stop the process
- **Cont** = Default action is to continue the process

---

## 23. Glossary

| Term | Definition |
|---|---|
| **Signal** | An asynchronous notification sent to a process to notify it of an event. |
| **SIGSTOP** | A signal that stops a process. Cannot be caught or ignored. |
| **SIGCONT** | A signal that resumes a stopped process. Cannot be blocked. |
| **SIGTSTP** | Terminal stop signal (Ctrl+Z). Can be caught or ignored. |
| **SIGKILL** | A signal that terminates a process. Cannot be caught or ignored. |
| **SIGTERM** | A signal that requests graceful termination. Can be caught. |
| **SIGHUP** | Hangup signal. Sent when terminal disconnects or to request daemon reload. |
| **Job** | A pipeline of processes managed by the shell as a unit. |
| **Process Group** | A collection of related processes. Used for signal distribution. |
| **Session** | A collection of process groups. Typically associated with a login session. |
| **Controlling Terminal** | The terminal associated with a session. Generates signals for foreground process group. |
| **Foreground Process** | A process (group) that receives input from the terminal. |
| **Background Process** | A process running without terminal input. |
| **Zombie** | A terminated process whose exit status has not been collected by its parent. |
| **Orphan** | A process whose parent has terminated. Reparented to PID 1. |
| **Daemon** | A long-running background process with no controlling terminal. |
| **ptrace** | A system call that allows one process to observe and control another. |
| **cgroup** | Control group — a kernel feature for limiting and accounting for resources. |
| **Freezer** | A cgroup subsystem that can atomically freeze and thaw all processes in a group. |
| **CRIU** | Checkpoint/Restore In Userspace — a tool for saving and restoring process state. |
| **WCHAN** | The kernel function (wait channel) where a process is sleeping. |
| **Async-signal-safe** | Functions that can be safely called from a signal handler. |
| **Signal Mask** | A set of signals that a process has blocked (deferred delivery). |
| **Signal Disposition** | What the kernel does when a signal is delivered (default, catch, ignore). |
| **D State** | Uninterruptible sleep. Process is waiting for I/O and cannot be stopped or killed. |
| **T State** | Stopped. Process has received SIGSTOP, SIGTSTP, SIGTTIN, or SIGTTOU. |
| **t State** | Traced. Process is being debugged via ptrace. |
| **jobspec** | A shell reference to a job (e.g., `%1`, `%+`, `%string`). |
| **nohup** | A command/utility that makes a process immune to SIGHUP. |
| **disown** | A shell builtin that removes a job from the shell's job table. |
| **setsid** | Creates a new session, detaching from the controlling terminal. |
| **YAMA** | A Linux Security Module that restricts ptrace access. |
| **OOM Killer** | Out-Of-Memory killer — terminates processes when system memory is exhausted. |

---

## Further Reading

- `man 7 signal` — Signal overview and behavior
- `man 2 kill` — kill() system call
- `man 2 sigaction` — Signal handling API
- `man 2 ptrace` — Process tracing
- `man 7 job-control` — Job control overview (bash)
- `man 1 bash` — Section on JOB CONTROL
- `man 5 proc` — /proc filesystem
- `man 1 strace` — System call tracer
- `man 1 gdb` — GNU Debugger
- `man 1 criu` — Checkpoint/Restore
- `man 1 systemctl` — systemd service management
- `Documentation/admin-guide/cgroup-v2.rst` — Kernel cgroup v2 docs
- `Documentation/admin-guide/cgroup-v1/freezer.rst` — Kernel freezer docs
- "The Linux Programming Interface" by Michael Kerrisk — Chapters 20-23 (Signals), Chapter 26 (Monitoring Child Processes)
- "Advanced Programming in the UNIX Environment" by W. Richard Stevens — Chapters 10-11 (Signals, Daemons)

---

### Thank you 
Contact if you want to join our classes/support: 
[EFXTV](https://t.me/efxtv)

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/efxtv)
