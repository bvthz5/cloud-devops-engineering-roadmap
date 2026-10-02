# 08 — Systemd Service Failures and Crash Loops

## 1. Scenario
A custom systemd service `my-api.service` enters a `failed` (Result: start-limit-hit) crash loop state upon server boot or deployment.

```text
Scenario
   ↓
Symptoms: systemctl status shows "Active: failed (Result: exit-code)", "start-limit-hit"
   ↓
What could cause it? Incorrect ExecStart binary path, missing environment file, wrong WorkingDirectory
   ↓
Diagnostic commands: systemctl status my-api, journalctl -u my-api -b --no-pager -n 50
   ↓
Find root cause: ExecStart path /usr/bin/node is invalid (binary is in /usr/local/bin/node)
   ↓
Fix / mitigate: Update unit file, run systemctl daemon-reload, restart service
```

## 2. Diagnostic Investigation Steps

```bash
# 1. Inspect unit status summary
systemctl status my-api.service

# 2. Inspect full journal logs for target unit since current boot
journalctl -u my-api.service -b --no-pager -n 50

# 3. Validate systemd unit syntax
systemd-analyze verify /etc/systemd/system/my-api.service
```

### Sample Status Output Analysis
```text
● my-api.service - Node.js API Service
     Loaded: loaded (/etc/systemd/system/my-api.service; enabled; vendor preset: enabled)
     Active: failed (Result: exit-code) since Thu 2026-09-17 21:10:00 UTC; 12s ago
    Process: 14210 ExecStart=/usr/bin/node /opt/api/server.js (code=exited, status=203/EXEC)
```
- **`status=203/EXEC`**: Systemd failed to execute binary `/usr/bin/node` (file missing or permission denied!).

## 3. Common Systemd Exit Code Reference

| Exit Code | Systemd Error Meaning | Fix Action |
|---|---|---|
| `200/CHDIR` | Failed to change to `WorkingDirectory`. | Verify directory path exists (`mkdir -p`). |
| `203/EXEC` | Failed to execute binary in `ExecStart`. | Verify binary path (`which node`). |
| `208/STDIN` | Failed to setup file descriptor / log stream. | Check `StandardOutput` log path permissions. |
| `217/USER` | Target `User=` or `Group=` does not exist. | Create service user account (`useradd -r`). |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Windows CRLF Bugs](./07-Windows-CRLF-Line-Ending-Problems.md) | [README](./README.md) | [09 - Port Conflicts & Socket Failures](./09-Port-Conflicts-and-Socket-Binding-Failures.md) |
