# 04 — copytruncate vs create & Signal Delivery

Understanding how daemons interact with Linux open file descriptors is the difference between seamless log rotation and catastrophic disk space leaks.

---

## 1. How File Descriptors Work During Rotation

When an application opens `/var/log/app.log`, the Linux kernel assigns a **file descriptor (FD)** pointing to the file's underlying **Inode**.

### Method 1: The Rename + Signal Approach (`create` + `postrotate`)
1. Logrotate renames `app.log` to `app.log.1`. The application continues writing to `app.log.1` uninterrupted because its open FD still points to the same Inode.
2. Logrotate creates a brand new, empty `app.log`.
3. In `postrotate`, logrotate sends a signal to the daemon:
   ```bash
   kill -USR1 $(cat /var/run/nginx.pid)  # Or kill -HUP
   ```
4. The daemon catches the signal, closes the old FD, opens the new `app.log`, and resumes writing.
- **Advantage:** **Zero data loss.** 100% reliable for software that supports signal reloads (NGINX, Apache, rsyslog).

---

### Method 2: The `copytruncate` Approach
Used for applications (such as Node.js, Go, or Java apps) that **do not support signal reloading**:
1. Logrotate makes a copy of `app.log` to `app.log.1`.
2. Logrotate truncates `app.log` in place to 0 bytes using `truncate` system call (`: > app.log`).
3. The application continues writing to the same open FD at offset 0.
- **Caveat:** There is a tiny race condition between copying the file and truncating it; log lines written during that microsecond window may be lost.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Logrotate Configuration and Retention Policies](./03-Logrotate-Configuration-and-Retention-Policies.md) | [Index](../../../README.md) | [05 - Centralized Log Aggregation Shippers →](./05-Centralized-Log-Aggregation-Shippers.md) |
