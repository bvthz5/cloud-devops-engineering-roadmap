# 14 - Hands-On Practice & Terminal Labs

Reinforce operating systems theory with live hands-on experiments on your Linux terminal.

---

## 🧪 Lab 1: Inspecting the Process Control Block (PCB) via `/proc`

### Objective:
Directly inspect the Linux kernel's internal `task_struct` metadata for a running process.

```bash
# 1. Start a sleep process in the background:
sleep 600 &
SLEEP_PID=$!
echo "Process PID is: $SLEEP_PID"

# 2. Inspect its process status and state:
cat /proc/$SLEEP_PID/status | grep -E "Name|State|Pid|PPid|Threads|VmSize|VmRSS"

# OBSERVE:
# - State: S (sleeping)
# - PPid: Matches your current shell ($$)
# - VmSize (VSZ) vs VmRSS (Physical RAM)

# 3. View its parent-child tree relationship:
pstree -p -s $SLEEP_PID

# 4. Clean up:
kill $SLEEP_PID
```

---

## 🧪 Lab 2: Observing Minor vs. Major Page Faults

### Objective:
See how the OS distinguishes between page faults resolved from RAM (Minor) vs disk I/O (Major).

```bash
# 1. Run a command under GNU time with verbose memory instrumentation:
/usr/bin/time -v ls -la /usr/bin > /dev/null

# 2. Look closely at the output metrics:
# - Minor (reclaiming a frame) page faults: ~100 to 500
# - Major (requiring I/O) page faults: 0 (if cached) or > 0 (if read from disk)
# - Maximum resident set size (kbytes): Real peak physical RAM consumed
```

---

## 🧪 Lab 3: Creating and Reaping a Zombie Process

### Objective:
Create a zombie process to observe its state in `ps`, and watch how PID 1 handles it.

```bash
# 1. Write a mini Python script that spawns a child that dies immediately:
python3 -c '
import os, time
pid = os.fork()
if pid == 0:
    print("Child exiting now...")
    os._exit(0)
else:
    print(f"Parent PID {os.getpid()} sleeping for 20s without calling wait()...")
    time.sleep(20)
' &

# 2. Immediately inspect ps for zombie state:
ps aux | grep -E "Z|defunct" | grep -v grep

# OBSERVE:
# Output shows: [python3] <defunct> with STAT "Z+"!
# It consumes 0% CPU and 0% MEM!

# 3. Wait 20 seconds for the parent to terminate:
# As soon as parent terminates, the zombie vanishes!
```

---

## 🧪 Lab 4: Tracking Hardware Interrupts & SoftIRQs

### Objective:
Observe real-time electrical hardware interrupts and software interrupts across your CPU cores.

```bash
# 1. View hardware interrupt distribution across CPU cores:
cat /proc/interrupts | head -n 15

# Look at:
# - Timer interrupts (LOC)
# - Storage disk interrupts (nvme or ahci)
# - Network interface interrupts (eth0/wlan0)

# 2. View software interrupt counters:
cat /proc/softirqs
# Look at NET_RX (network packet receive processing) and SCHED (CPU timer scheduling)
```
