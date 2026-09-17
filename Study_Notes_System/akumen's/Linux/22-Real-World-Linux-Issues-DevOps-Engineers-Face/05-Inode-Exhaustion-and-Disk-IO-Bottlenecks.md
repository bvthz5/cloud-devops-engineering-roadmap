# 05 — Inode Exhaustion and Disk I/O Bottlenecks

## 1. Scenario: Inode Allocation Exhaustion
Applications report `No space left on device` when creating new files, but `df -h` shows **50 GB free disk space**!

```text
Scenario
   ↓
Symptoms: "No space left on device" errors despite plenty of gigabytes free in df -h
   ↓
What could cause it? 100% Inode utilization (allocation table exhausted by millions of tiny files)
   ↓
Diagnostic commands: df -i, find /var/spool -type f | wc -l
   ↓
Find root cause: Sendmail mail queue or PHP session directory contains 5 million 0-byte files
   ↓
Fix / mitigate: Delete accumulated files using find -delete or xargs rm
   ↓
Verify: df -i shows IFree restored
```

### Diagnosing Inode Usage (`df -i`)
```bash
df -i /
```

```text
Filesystem     Inodes  IUsed  IFree IUse% Mounted on
/dev/sda1     3276800 3276800     0  100% /
```

### Finding Directory Holding Excessive Small Files
```bash
# Count files per top-level directory in /var:
for d in /var/*; do echo "$d: $(find "$d" | wc -l)"; done | sort -t: -k2 -nr
```

### Safe Fast Deletion of Millions of Files
```bash
# Standard 'rm *' will fail with "Argument list too long". Use find with -delete:
find /var/spool/clientmqueue/ -type f -delete
```

---

## 2. Scenario: Disk I/O Latency Bottlenecks
Application requests hang while CPU usage shows low `%us` but very high `%wa` (I/O Wait).

### Diagnostic Tools
```bash
# 1. Extended device stats (check %util, await, r/s, w/s)
iostat -xz 1 5

# 2. Identify top processes generating disk read/write bandwidth
iotop -oPa
```

### Sample `iostat -xz 1` Output Analysis
```text
Device            r/s     w/s     rMB/s     wMB/s   await  %util
sda            150.00  950.00      4.20     48.50   45.20  99.80
```
- **`%util 99.80%`**: Device `sda` is fully saturated.
- **`await 45.20 ms`**: Average time disk requests wait for service is 45ms (normal is < 5ms).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Disk Space Leak](./04-Disk-Space-Exhaustion-and-Deleted-Open-Files.md) | [README](./README.md) | [06 - Permissions & PATH Issues](./06-Permissions-Ownership-and-Path-Resolution-Issues.md) |
