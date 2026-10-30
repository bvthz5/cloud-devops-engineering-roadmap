# 08 — SSH Troubleshooting Guide & Runbook

---

## 1. Deep Debugging with `-vvv`

```bash
ssh -vvv user@server.com
```
Look for:
- `Offering public key`: local key was submitted.
- `Permission denied (publickey)`: Server rejected key or `.ssh` permissions are invalid.
- Reset host key: `ssh-keygen -R server.com`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
