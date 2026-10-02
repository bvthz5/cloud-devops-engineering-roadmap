# 09 — Linux Logging Interview Q&A

10 technical interview questions for DevOps, SRE, and Infrastructure roles.

---

### Q1: What is the technical difference between `copytruncate` and standard rotation with `create` in logrotate?
**Answer:**
- In standard rotation with `create`, logrotate renames the original file to `app.log.1` (preserving its Inode), creates a new empty `app.log`, and issues a signal (like `SIGHUP` or `SIGUSR1`) to the daemon in `postrotate` telling it to reopen its file descriptor. This is 100% atomic and prevents data loss.
- In `copytruncate`, logrotate copies the file contents to `app.log.1` and then truncates the existing file in place to 0 bytes (`: > app.log`). The daemon does not need to close its file descriptor. However, log lines written during the microsecond gap between the copy and truncate operations can be lost.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
