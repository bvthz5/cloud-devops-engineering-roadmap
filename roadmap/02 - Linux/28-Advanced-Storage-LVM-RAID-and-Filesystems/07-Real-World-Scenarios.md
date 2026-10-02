# 07 — Real-World Storage Production Scenarios

---

## Scenario 1: Disk Space at 100%, But `du` Doesn't Match `df`

### Incident Summary
Monitoring triggers: `/var/log` is at 100% disk usage. The database crashes.
The SRE runs `du -sh /var/log/*` and the sum of all files is only 2 GB, but `df -h` shows 50 GB used! Where is the phantom space?

### Root Cause Analysis
A developer or script ran `rm -f /var/log/application.log`.
However, an active Java or NGINX process still had an open file descriptor pointing to that file!
In Linux, **deleting a file with `rm` only removes the directory entry (unlink)**. The disk blocks are NOT freed until every process holding an open file descriptor closes it.

### Production Solution
1. Identify the holding process and deleted file:
   ```bash
   sudo lsof +L1
   # Or:
   sudo lsof | grep '(deleted)'
   ```
2. Free the disk space immediately without restarting the service by truncating the file descriptor:
   ```bash
   # Find the PID and FD (e.g. PID 1240, FD 4)
   sudo : > /proc/1240/fd/4
   ```
   `df -h` will instantly show the freed space!

---

## Scenario 2: Inode Exhaustion (`No space left on device` with 80% free disk)

### Incident Summary
An application fails to write a new file with `No space left on device`.
`df -h` shows 200 GB available disk space!

### Root Cause Analysis
Filesystems have a fixed number of inodes created at format time. If an application (e.g. session cache or mail queue) creates millions of tiny 0-byte or 10-byte files, all available inodes are consumed while raw disk blocks remain empty.

### Production Solution
1. Verify inode usage:
   ```bash
   df -i
   # Output: /dev/sda1 100% inodes used!
   ```
2. Locate the directory hoarding millions of files:
   ```bash
   sudo find / -xdev -printf '%h
' | sort | uniq -c | sort -k 1 -n | tail -n 10
   ```
3. Purge the orphan files:
   ```bash
   sudo find /var/spool/clientmqueue/ -type f -delete
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Online Filesystem Expansion and Maintenance](./06-Online-Filesystem-Expansion-and-Maintenance.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
