# 16 — Hands-On Practice Labs: OS & Kernel Fundamentals

Interactive, command-driven laboratory exercises to inspect system calls, process state machines, signal traps, procfs telemetry, and cgroups resource limits.

---

## Lab 1: System Call Tracing with `strace`

### Objective
Observe how user space programs communicate with the Linux kernel through system calls.

### Commands to Execute
```bash
# 1. Trace all system calls made by the simple command 'cat /etc/os-release'
strace cat /etc/os-release

# 2. Count and aggregate time spent in each system call
strace -c ls -la /var/log

# 3. Filter only file-related system calls (openat, read, write, close)
strace -e trace=openat,read,write,close date

# 4. Attach strace to a running background process (e.g., cron or sshd)
sudo strace -p $(pgrep -x cron || pgrep -x crond || pgrep -x sshd | head -1) -s 64
```

### Analysis Checklist
- Notice how `execve` is always the first system call executed when a new binary starts.
- Identify the transition where `openat` returns an integer file descriptor (e.g., `3`), followed by `read(3, ...)`, and finalized with `close(3)`.

---

## Lab 2: Reproducing, Inspecting, and Reaping a Zombie Process

### Objective
Create a controlled zombie process in Python, observe its state in `ps`, understand why `kill -9` fails on it, and clean it up.

### Step 1: Create the Zombie Demonstration Script
Save to `/tmp/zombie_lab.py`:
```python
import os
import time

pid = os.fork()

if pid > 0:
    print(f"[Parent] Parent PID: {os.getpid()}, spawned child PID: {pid}")
    print("[Parent] Sleeping for 45 seconds without calling wait()...")
    time.sleep(45)
    print("[Parent] Waking up, calling wait() to reap child...")
    os.wait()
    print("[Parent] Child successfully reaped! Exiting.")
else:
    print(f"[Child] Child PID: {os.getpid()} exiting immediately.")
    os._exit(0)
```

### Step 2: Execute and Observe in Another Terminal
```bash
# Terminal 1: Run the script
python3 /tmp/zombie_lab.py

# Terminal 2: Inspect process state
ps -o pid,ppid,stat,comm -p $(pgrep -f zombie_lab.py)
```
- Observe the child process: status is **`Z`** (or `Z+`) and command shows `<defunct>`.

### Step 3: Attempt to Kill the Zombie
```bash
# In Terminal 2, attempt to kill child PID directly:
CHILD_PID=$(pgrep -f zombie_lab.py | tail -1)
kill -9 $CHILD_PID

# Check if it died:
ps -p $CHILD_PID
```
- **Finding:** The zombie is still there! You cannot kill a process that is already dead.
- Wait for the parent's 45-second sleep to end; observe how the parent's `wait()` immediately clears the zombie from the process table.

---

## Lab 3: Graceful Shutdown & Signal Trapping

### Objective
Write a production-style shell daemon that traps `SIGTERM` and `SIGINT` to perform clean shutdown routines.

### Step 1: Create the Script
Save to `/tmp/graceful_daemon.sh`:
```bash
#!/bin/bash

cleanup() {
    echo ""
    echo "[$(date +%T)] Received termination signal!"
    echo "[$(date +%T)] Closing database connections..."
    echo "[$(date +%T)] Flushing log buffers to disk..."
    sleep 2
    echo "[$(date +%T)] Clean shutdown complete. Exiting with code 0."
    exit 0
}

# Trap SIGTERM (15) and SIGINT (2)
trap cleanup SIGTERM SIGINT

echo "[$(date +%T)] Daemon started with PID: $$"
echo "[$(date +%T)] Waiting for work or signals..."

while true; do
    sleep 1
done
```

### Step 2: Test Signal Handling
```bash
# Make executable and run
chmod +x /tmp/graceful_daemon.sh
/tmp/graceful_daemon.sh &
DAEMON_PID=$!

# Send graceful termination signal (simulating Kubernetes pod termination)
kill -15 $DAEMON_PID
```
- Observe that the script intercepts the signal, completes the cleanup block, and exits cleanly.

---

## Lab 4: Exploring Process Internals via `/proc`

### Objective
Extract runtime telemetry from an active process without using high-level commands.

### Commands to Execute
```bash
# Launch a background process
sleep 300 &
PID=$!

# 1. View complete process command line arguments
cat /proc/$PID/cmdline | tr '\0' ' ' ; echo ""

# 2. View current process state, memory limits, and threads
cat /proc/$PID/status | grep -E "Name|State|Tgid|Pid|PPid|VmRSS|Threads"

# 3. View open file descriptors
ls -l /proc/$PID/fd/

# 4. View memory consumption breakdown
cat /proc/$PID/smaps_rollup | grep -E "Rss|Pss|Shared_Clean|Private_Dirty"

# Terminate test process
kill -9 $PID
```

---

## Lab 5: Enforcing Memory Limits with `cgroups v2`

### Objective
Create an isolated control group, configure a strict 50 MB memory ceiling, launch a memory-consuming process, and observe the kernel OOM killer in action.

### Commands to Execute (Requires Root)
```bash
# 1. Create a custom test cgroup
sudo mkdir -p /sys/fs/cgroup/memory-test

# 2. Set memory ceiling to 50 Megabytes
echo "50M" | sudo tee /sys/fs/cgroup/memory-test/memory.max

# 3. Move current subshell into this cgroup
echo $$ | sudo tee /sys/fs/cgroup/memory-test/cgroup.procs

# 4. Attempt to allocate 100 MB of RAM using python
python3 -c 'a = "A" * (100 * 1024 * 1024)'
```

### Expected Output
The shell immediately outputs:
```text
Killed
```
Run `dmesg -T | grep -i oom` to view the kernel OOM killer log confirming the process was terminated for exceeding the `memory-test` cgroup ceiling.

Clean up:
```bash
sudo rmdir /sys/fs/cgroup/memory-test
```

---

## Lab 6: Building and Managing a systemd Service

### Objective
Package a background script into a production systemd service with automated crash restart and logging.

### Commands to Execute
```bash
# 1. Create test script
sudo mkdir -p /opt/heartbeat
cat << 'EOF' | sudo tee /opt/heartbeat/heartbeat.sh
#!/bin/bash
while true; do
  echo "Heartbeat tick at $(date)"
  sleep 5
done
EOF
sudo chmod +x /opt/heartbeat/heartbeat.sh

# 2. Create systemd unit file
cat << 'EOF' | sudo tee /etc/systemd/system/heartbeat.service
[Unit]
Description=Heartbeat Test Service
After=network.target

[Service]
Type=simple
ExecStart=/opt/heartbeat/heartbeat.sh
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
EOF

# 3. Reload systemd, enable, and start
sudo systemctl daemon-reload
sudo systemctl enable --now heartbeat.service

# 4. Check status and stream logs
systemctl status heartbeat.service
journalctl -u heartbeat.service -f -n 10

# 5. Clean up
sudo systemctl stop heartbeat.service
sudo systemctl disable heartbeat.service
sudo rm /etc/systemd/system/heartbeat.service
sudo systemctl daemon-reload
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Interview QA](./15-Interview-QA.md) | [README](./README.md) | [17 - MCQ](./17-MCQ.md) |
