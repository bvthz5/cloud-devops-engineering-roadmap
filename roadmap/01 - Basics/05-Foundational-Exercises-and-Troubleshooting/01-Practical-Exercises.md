# Practical Foundation Exercises (Hands-On Labs 1 to 10)

## Exercise 1 — Identify Your Hardware Environment

Find CPU model, core counts, RAM, storage, and architecture.

### Linux Commands

```bash
lscpu
free -h
lsblk
lspci
uname -m
ip link
```

### Windows Commands

```powershell
systeminfo
wmic cpu get name, NumberOfCores, NumberOfLogicalProcessors
wmic computersystem get TotalPhysicalMemory
```

---

## Exercise 2 — Inspect Operating System & Kernel

```bash
uname -a
uname -r
hostname
whoami
echo $SHELL
cat /etc/os-release
```

---

## Exercise 3 — Process Inspection & Monitoring

```bash
# View all processes
ps aux

# Search for a specific daemon
ps aux | grep sshd

# Real-time resource monitor
top
```

---

## Exercise 4 — File Descriptors Deep Dive

```bash
# Inspect standard file descriptors of active shell
ls -l /proc/$$/fd

# Output:
# 0 -> /dev/pts/0 (stdin)
# 1 -> /dev/pts/0 (stdout)
# 2 -> /dev/pts/0 (stderr)
```

---

## Exercise 5 — PATH Variable Resolution

```bash
echo $PATH
which ls
which python3
which docker
type cd
type ll
```

---

## Exercise 6 — Redirection Mastery

```bash
echo "Hello World" > test.txt
echo "Cloud & DevOps" >> test.txt
cat test.txt
ls /nonexistent 2> error.log
cat error.log
```

---

## Exercise 7 — Pipeline Chaining

```bash
# Count total running processes matching 'root'
ps aux | grep root | wc -l
```

---

## Exercise 8 — Exit Code Verification

```bash
true
echo $? # Output: 0 (Success)

false
echo $? # Output: 1 (Failure)
```

---

## Exercise 9 — Handcraft Valid YAML Configuration

Create `app.yaml`:

```yaml
application:
  name: devops-microservice
  version: "1.0.0"
  port: 8080

database:
  engine: postgresql
  host: localhost
  port: 5432

services:
  - web
  - worker
  - cache
```

---

## Exercise 10 — YAML to JSON Conversion Verification

Convert `app.yaml` to JSON equivalent:

```json
{
  "application": {
    "name": "devops-microservice",
    "version": "1.0.0",
    "port": 8080
  },
  "database": {
    "engine": "postgresql",
    "host": "localhost",
    "port": 5432
  },
  "services": [
    "web",
    "worker",
    "cache"
  ]
}
```
