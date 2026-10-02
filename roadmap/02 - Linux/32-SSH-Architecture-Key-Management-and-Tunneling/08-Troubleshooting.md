# 08 — SSH Troubleshooting Guide & Runbook

---

## 1. Deep Debugging with `-vvv`

When an SSH connection hangs or fails authentication:
```bash
ssh -vvv user@server.com
```
Look for:
- `Authentications that can continue`: Lists server-supported auth methods.
- `Offering public key`: Shows which local key was offered and whether the server rejected it.
- `Connection timed out`: Indicates firewall or security group blocking port 22.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
