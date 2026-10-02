# 09 — systemd Interview Q&A

10 technical interview questions for DevOps, SRE, and Infrastructure Engineering roles.

---

### Q1: What is the difference between `systemctl reload` and `systemctl restart`?
**Answer:**
- `systemctl reload`: Instructs the service to re-read its configuration files on the fly (typically by sending a `SIGHUP` signal) without terminating the process or interrupting active client connections. Not all services support reload.
- `systemctl restart`: Terminates the existing process (`SIGTERM` followed by `SIGKILL` if necessary) and creates a completely new process. This causes a brief interruption to active connections.

---

### Q2: How does systemd prevent processes from escaping supervision when they daemonize or fork?
**Answer:**
Legacy SysVinit tracked processes using PID files (`/var/run/*.pid`). If a rogue process double-forked, it escaped tracking and became an untracked zombie.
systemd places every service into its own dedicated **Linux Control Group (cgroup)**. Because child processes automatically inherit the parent's cgroup regardless of how many times they fork or rename themselves, systemd maintains 100% reliable tracking. When you execute `systemctl stop`, systemd terminates every process inside that cgroup via `KillMode=control-group`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
