# 25 — DevOps Troubleshooting Interview Questions

## Q1: A Linux server is slow. What are the first 5 commands you run to diagnose it?
**Answer**:
1. `uptime`: Check load average (compare 1, 5, 15 min averages against CPU core count `nproc`).
2. `top` / `htop`: Check CPU breakdown (`%us`, `%sy`, `%wa`) and identify top CPU/Memory processes.
3. `free -h`: Inspect RAM availability, buffer/cache allocations, and swap space paging.
4. `df -h` & `df -i`: Verify filesystem disk capacity and inode allocation limits.
5. `dmesg -T | tail -n 30`: Check kernel ring buffer for hardware, disk I/O errors, or OOM-killer invocations.

---

## Q2: `df -h` shows disk space is 100% full, but `du -sh` shows only 10 GB used out of 100 GB. What is happening and how do you fix it?
**Answer**:
Files have been deleted (`rm`) from directory indexes, but active processes still hold open file descriptors to those files. The kernel cannot reclaim the underlying disk storage blocks until the process releases the file handle.
- **Diagnosis**: Run `lsof +L1` or `lsof | grep deleted` to identify process PIDs and file descriptor (FD) numbers.
- **Fix**: Truncate the file descriptor directly via `> /proc/PID/fd/FD` or reload/restart the process holding the file handle.

---

## Q3: How do you troubleshoot a Kubernetes pod stuck in `CrashLoopBackOff`?
**Answer**:
1. Run `kubectl get pod <pod-name> -o wide` to check restart count and assigned node.
2. Run `kubectl describe pod <pod-name>` to inspect events, container exit status codes, and probe failures.
3. Run `kubectl logs <pod-name> --previous` to inspect stdout/stderr log output of the container instance immediately prior to crashing.
4. If container exits instantly due to missing config, inspect secrets, environment variables, and entrypoint binary path.

---

## Q4: What is the difference between Load Average and CPU Utilization?
**Answer**:
- **CPU Utilization** measures the percentage of CPU clock cycles actively executing instructions (`%us` user, `%sy` system).
- **Load Average** measures the average number of threads in an **executable or uninterruptible state** (queued waiting for CPU core execution or waiting for disk/network I/O). A system can have 10% CPU utilization but a load average of 20 if processes are blocked on disk I/O (`%wa`).

---

## Q5: How do you fix an SSH `Permission denied (publickey)` error?
**Answer**:
1. Run `ssh -vvv user@host` to trace client key offerings and server responses.
2. Inspect target user home and SSH directory permissions on server:
   - `~` home directory: `755` or `700`
   - `~/.ssh` directory: `700`
   - `~/.ssh/authorized_keys`: `600`
   - `~/.ssh/id_rsa` private key: `600`
3. Verify server `~/.ssh/authorized_keys` contains the exact public key corresponding to client private key.
4. Check `/var/log/auth.log` or `journalctl -u sshd` for server-side refusal logs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [24 - Scenario Drills](./24-Real-World-Scenario-Drills-01-to-07.md) | [README](./README.md) | [26 - Scenario MCQs](./26-Scenario-Based-MCQs-and-Diagnostic-Quizzes.md) |
