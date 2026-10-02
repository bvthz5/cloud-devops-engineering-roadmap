# OS & Kernel Fundamentals — Practical Commands & Diagnostic Scenarios

## 1. Inspecting Kernel Version & OS Details

```bash
# Display kernel release, architecture, and build timestamp
uname -a

# View OS distribution release details
cat /etc/os-release
```

---

## 2. Inspecting Processes & PIDs

```bash
# Snapshot of all running processes with user, PID, CPU, Memory
ps aux

# View tree representation of parent-child process hierarchy
pstree -p

# Interactive real-time process & resource viewer
top -b -n 1 | head -n 20
```

---

## 3. Tracing System Calls with `strace`

```bash
# Trace system calls of a command (e.g. ls)
strace -c ls -la

# Attach strace to an active running process PID
sudo strace -p 1234 -e trace=open,read,write
```

---

## 4. Inspecting File Descriptors with `lsof` and `/proc`

```bash
# List all open file descriptors of a process PID
lsof -p 1234

# View standard file descriptors for current shell
ls -l /proc/$$/fd
```

---

## 5. Inspecting Kernel Modules & Kernel Ring Buffer

```bash
# List all loaded kernel modules
lsmod | head -n 15

# View kernel ring buffer log messages (hardware events, OOM events)
sudo dmesg -T | tail -n 25
```
