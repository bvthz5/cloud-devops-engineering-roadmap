# 16 — Suspicious Processes and Security Incident Response

## 1. Scenario
CPU usage reaches 100% on a server due to an unfamiliar process (e.g. `kdevtmpf` or `kworker/u:2-events`) running out of `/tmp` or `/var/tmp`.

```text
Scenario
   ↓
Symptoms: 100% CPU usage, process running from /tmp, unfamiliar process name, rogue cron persistence
   ↓
What could cause it? Cryptominer infection or compromised web app shell execution
   ↓
Diagnostic commands: ps aux, ls -l /proc/PID/exe, lsof -p PID, crontab -l, cat /etc/crontab
```

## 2. Inspecting Process Binary & Environment (`/proc/PID/`)

```bash
# 1. Identify suspicious PID
ps aux --sort=-%cpu | head -n 5

# 2. Find absolute executable binary path on disk
ls -l /proc/14520/exe

# 3. Inspect working directory and environment variables
ls -l /proc/14520/cwd
cat /proc/14520/environ | tr '\0' '\n'

# 4. Inspect active network sockets held by malware PID
lsof -i -p 14520
```

## 3. Investigating Persistence Mechanisms
Malware re-spawns using automated cron tasks or systemd timer hooks:

```bash
# Inspect system crontab files
crontab -l
cat /etc/crontab
ls -la /etc/cron.*/ /var/spool/cron/crontabs/

# Inspect user systemd timers
systemctl list-timers --all
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Kubernetes Pod CrashLoopBackOff](./15-Kubernetes-Pod-CrashLoopBackOff-Troubleshooting.md) | [README](./README.md) | [17 - Universal Troubleshooting Toolkit](./17-Universal-Troubleshooting-Toolkit-and-Cheat-Sheet.md) |
