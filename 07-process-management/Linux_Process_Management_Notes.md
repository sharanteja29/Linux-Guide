# Linux Process Management — CE-Focused Notes

## 1. What Is a Process?

A **process** is an instance of a program that is currently running.

Example:

```bash
python app.py
```

When `app.py` runs, Linux creates a process for it. Each process has a **PID (Process ID)**.

### Parent and Child Processes

Processes can create other processes:

```text
Parent Process
      |
      +---- Child Process
      |
      +---- Child Process
```

---

# 2. Viewing Processes

## `ps aux`

Shows a broad list of running processes.

```bash
ps aux
```

Important columns:

- `USER` — user running the process
- `PID` — process ID
- `%CPU` — CPU usage
- `%MEM` — memory usage
- `COMMAND` — command/program

### CE use

Useful when investigating what is running on a server and which process may be consuming resources.

## `ps -u username`

Shows processes belonging to a particular user.

```bash
ps -u alice
```

## `ps -C processname`

Finds processes by program name.

```bash
ps -C python
```

## `pgrep processname`

Finds the PID of processes matching a name.

```bash
pgrep python
```

Example:

```text
1250
1372
```

## `pidof processname`

Finds the PID of a program.

```bash
pidof python
```

---

# 3. Terminating Processes

## `kill PID`

Sends a signal to a process.

```bash
kill 1250
```

By default, `kill` normally sends **SIGTERM**, which asks the process to terminate gracefully.

```text
kill PID
   ↓
"Please shut down cleanly."
```

## `kill -9 PID`

Forcefully terminates a process.

```bash
kill -9 1250
```

`-9` means **SIGKILL**.

Use this when a process does not respond to normal termination. Prefer graceful termination first.

## `pkill processname`

Kills processes based on their name.

```bash
pkill python
```

This can affect multiple matching processes, so use it carefully.

## `pkill -9 processname`

Force kills matching processes.

```bash
pkill -9 python
```

---

# 4. Stop and Resume a Process

## `kill -STOP PID`

Suspends a process.

```bash
kill -STOP 1250
```

The process remains in memory but is stopped from executing.

## `kill -CONT PID`

Continues a process that was stopped.

```bash
kill -CONT 1250
```

Think:

```text
Running
   ↓
STOP
   ↓
Stopped
   ↓
CONT
   ↓
Running
```

STOP/CONT are not the same as terminate/restart.

---

# 5. Foreground and Background Processes

A normal command runs in the foreground:

```bash
sleep 100
```

The terminal waits for it to finish.

## Start directly in the background

Add `&`:

```bash
sleep 100 &
```

The terminal becomes available immediately.

## `jobs`

Shows jobs managed by the current shell.

```bash
jobs
```

Example:

```text
[1]+  Running    sleep 100 &
```

`1` is the job number.

### Jobs vs Processes

- `jobs` → shell jobs
- `ps` → system process information

## `Ctrl + Z`

When a command is running in the foreground, press:

```text
Ctrl + Z
```

This suspends it.

Important: **Ctrl+Z does not directly put the process in the background.** It first stops/suspends it.

## `bg`

Resumes a stopped job in the background.

```bash
bg %1
```

## `fg`

Brings a background job into the foreground.

```bash
fg %1
```

### Complete flow

```text
Foreground
    |
    | Ctrl + Z
    v
Stopped
    |
    | bg %1
    v
Background
    |
    | fg %1
    v
Foreground
```

---

# 6. Process Priority

Linux gives processes a scheduling priority represented by a **nice value (NI)**.

General rule:

- Higher nice value → lower scheduling priority
- Lower nice value → higher scheduling priority
- Negative nice values generally require elevated privileges

## `nice`

Starts a new command with a specified nice value.

```bash
nice -n 10 python backup.py
```

The new process starts with lower scheduling priority than normal.

## `renice`

Changes the priority of an existing process.

```bash
renice -n 10 -p 1250
```

Difference:

```text
nice
 ↓
Set priority when starting a process

renice
 ↓
Change priority of an existing process
```

## Increase priority

```bash
renice -n -5 -p 1250
```

A negative nice value means higher scheduling priority and normally requires appropriate privileges.

---

# 7. Monitoring Processes

## `top`

Provides a live view of processes and resource usage.

```bash
top
```

It can show:

- CPU usage
- Memory usage
- PID
- Process owner
- Process state
- Process priority

Common keys inside `top`:

```text
k → kill a process
r → change process priority
q → quit
```

## `htop`

An interactive process monitor.

```bash
htop
```

It provides a more user-friendly interface for viewing and managing processes. It may not be installed on a minimal Linux system.

---

# 8. Services and Processes

A **process** is a running instance of a program.

A **service** is a system component managed as a background service, commonly through `systemd`.

Example:

```text
Service: ssh.service
        |
        | managed by systemd
        v
Process: sshd
        |
        v
PID: 1250
```

### Process commands

```bash
ps
pgrep
pidof
top
kill
```

### Service commands

```bash
systemctl status
systemctl start
systemctl stop
systemctl enable
```

A service can manage or supervise processes, including restarting them when configured to do so.

---

# 9. Service Management with `systemctl`

## Identify services

```bash
systemctl list-units --type=service
```

Lists currently loaded service units.

## Check service state

```bash
systemctl status ssh
```

Example:

```text
Active: active (running)
```

or:

```text
Active: inactive (dead)
```

## Start a service

```bash
sudo systemctl start ssh
```

Starts the service **now**.

## Stop a service

```bash
sudo systemctl stop ssh
```

Stops the service **now**.

## Enable a service

```bash
sudo systemctl enable ssh
```

Configures the service to start automatically during boot.

### Important distinction

```text
start
  ↓
Run now

enable
  ↓
Start automatically at boot
```

`enable` does not necessarily mean the service starts immediately.

## Disable a service

```bash
sudo systemctl disable ssh
```

Prevents the service from being configured to start automatically at boot.

It does not necessarily stop a currently running service.

## Start now + enable at boot

```bash
sudo systemctl enable --now ssh
```

This both starts the service now and enables it for boot.

---

# 10. Process vs Service

| Process | Service |
|---|---|
| Running instance of a program | Managed system component |
| Has a PID | Has a service/unit name |
| Investigated with `ps`, `pgrep`, `top` | Managed with `systemctl` |
| Can be killed | Can be started/stopped/enabled |
| Represents runtime execution | Represents managed application/system functionality |

### Simple mental model

```text
Process → "What is running?"

Service → "What system component am I managing?"
```

---

# 11. Real Cloud Engineer Troubleshooting Workflow

```text
Problem reported
      ↓
Check CPU / memory
      ↓
Identify process
      ↓
Find PID
      ↓
Inspect process
      ↓
Decide action
      ↓
Graceful termination/restart if required
      ↓
Verify recovery
```

Typical commands:

```bash
top
ps aux
pgrep <process-name>
kill <PID>
```

If the application is managed as a service:

```bash
systemctl status <service>
systemctl restart <service>
```

Then verify again.

---

# 12. CE Priority

## Must know very well

- Process vs program
- PID
- Parent/child processes
- `ps aux`
- `pgrep`
- `pidof`
- `kill`
- SIGTERM vs SIGKILL
- `kill -STOP` / `kill -CONT`
- Foreground vs background
- `&`
- `jobs`
- `Ctrl+Z`
- `bg`
- `fg`
- `top`
- `nice` vs `renice`
- Service vs process
- `systemctl status`
- `systemctl start`
- `systemctl stop`
- `systemctl enable`
- `start` vs `enable`

## Know reasonably

- `ps -u`
- `ps -C`
- `pkill`
- `htop`
- Process priority/nice values
- Basic process tree concepts

## Don't over-invest

For Cloud Engineer preparation, you do not need to memorize every Linux signal or every interactive `top` shortcut.

Focus on being able to diagnose a real server problem.

---

# 13. Quick Cheat Sheet

```text
PROCESS
ps aux                     → view processes
ps -u alice                → user's processes
ps -C python               → process by name
pgrep python               → find PID
pidof python               → find PID

PROCESS CONTROL
kill PID                   → graceful termination request
kill -9 PID                → force kill
pkill python               → kill by name
kill -STOP PID             → suspend
kill -CONT PID             → resume

BACKGROUND / FOREGROUND
command &                  → start in background
jobs                       → view shell jobs
Ctrl + Z                   → suspend foreground job
bg %1                      → resume in background
fg %1                      → bring to foreground

PRIORITY
nice -n 10 command         → start with lower priority
renice -n 10 -p PID        → change existing process priority

MONITORING
top                        → live process monitoring
htop                       → interactive process monitoring

SERVICES
systemctl status ssh       → check service
systemctl start ssh        → start now
systemctl stop ssh         → stop now
systemctl enable ssh       → enable at boot
systemctl disable ssh      → disable at boot
systemctl enable --now ssh → start now + enable at boot
```

# 14. Key Concepts

```text
Program
   ↓
Running program = Process
   ↓
Process gets a PID

Service
   ↓
Managed system component
   ↓
Usually managed by systemd/systemctl
```

Most important distinction:

```text
PROCESS
"What is running?"

SERVICE
"What system component am I managing?"

start
"What should run now?"

enable
"What should automatically run after boot?"
```
