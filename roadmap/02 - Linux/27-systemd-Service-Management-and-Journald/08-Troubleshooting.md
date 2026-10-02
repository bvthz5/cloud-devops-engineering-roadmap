# 08 — systemd Troubleshooting Guide & Runbook

Diagnostic methodologies for resolving service startup failures and performance issues.

---

## 1. Analyzing Service Failures

When a service fails to start or crashes:
```bash
# 1. View high-level exit status and recent log lines
systemctl status <service-name>

# 2. Inspect full debug logs with systemd execution details
sudo journalctl -xeu <service-name>

# 3. Check exact exit code meaning:
# Exit code 127: Binary / command not found
# Exit code 126: Permission denied / binary not executable
# Exit code 1: Application thrown exception
# Status 203/EXEC: Failed to execute ExecStart path or bad shebang (#!)
```

---

## 2. Safe Customization with Drop-in Overrides

**Never edit vendor files in `/lib/systemd/system/`!** Use drop-in overrides:
```bash
# Opens an editor and automatically creates /etc/systemd/system/<service>.service.d/override.conf
sudo systemctl edit <service-name>
```

To modify a directive like `ExecStart` (which must be cleared first before reassignment):
```ini
[Service]
# Clear existing command
ExecStart=
# Set new customized command
ExecStart=/usr/bin/my-custom-binary --new-flag
```

---

## 3. Investigating Slow Boot Times

```bash
# View total time spent in kernel, initrd, and userspace
systemd-analyze

# Identify the exact services causing the slowest boot delays (blame list)
systemd-analyze blame

# Generate an SVG visual waterfall diagram of boot sequence
systemd-analyze plot > boot-timeline.svg
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
