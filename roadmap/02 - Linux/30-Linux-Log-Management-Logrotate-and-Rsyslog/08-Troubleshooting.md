# 08 — Logrotate & Logging Troubleshooting Guide

---

## 1. Testing and Debugging Logrotate

Never wait 24 hours to see if your logrotate configuration works. Test it immediately!

```bash
# Debug Mode (Dry run: prints exact actions without modifying any files!)
sudo logrotate -d /etc/logrotate.d/nginx

# Verbose Mode (Executes rotation immediately, printing detailed steps)
sudo logrotate -v /etc/logrotate.d/nginx

# Force Immediate Rotation (Even if rotation criteria are not yet met)
sudo logrotate -f /etc/logrotate.d/nginx
```

---

## 2. Inspecting Logrotate State

Logrotate records the exact timestamp when each file was last rotated in `/var/lib/logrotate/status`:
```bash
cat /var/lib/logrotate/status
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
