# 07 — Real-World Performance Production Scenarios

---

## Scenario 1: Load Average at 80, CPU at 2% (The Uninterruptible Sleep Mystery)

### Incident Summary
Monitoring triggers P1 incident: Node load average has spiked to 85 on an 8-core server.
The on-call engineer checks `top` and sees `%usr: 1.5%` and `%idle: 98%`. CPU is practically idle! Why is load average 85?

### Root Cause Analysis
Load average includes processes in `D` state (Uninterruptible Sleep).
A thread enters `D` state when waiting for an uninterruptible kernel system call—almost always **disk I/O or a hanging NFS mount**.

### Production Diagnostic & Solution
1. Identify all processes stuck in `D` state:
   ```bash
   ps -eo pid,user,state,wchan:25,cmd | grep "^ *[0-9]* *[^ ]* *D"
   ```
2. Look at the `wchan` (wait channel) column. If it shows `nfs_wait_on_request` or `sync_inodes`, storage is stalled.
3. Check `dmesg -T` for disk controller timeouts or dead NFS servers.
4. Unmount the dead NFS mount forcefully:
   ```bash
   sudo umount -f -l /mnt/dead_share
   ```
   Load average will immediately drop from 85 back to 0.5!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Linux Kernel Tuning with sysctl](./06-Linux-Kernel-Tuning-with-sysctl.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
