# 12 — Quick Revision Cheat Sheet: systemd & journald

---

## 1. Core Command Cheat Sheet

```bash
# Service Lifecycle
systemctl start <unit>
systemctl stop <unit>
systemctl restart <unit>
systemctl reload <unit>
systemctl status <unit>

# Boot Automation
systemctl enable <unit>
systemctl disable <unit>
systemctl is-active <unit>
systemctl is-enabled <unit>

# Configuration Updates
sudo systemctl daemon-reload
sudo systemctl edit <unit>    # Create / edit drop-in override

# Log Inspection
journalctl -u <unit> -f       # Follow live logs
journalctl -u <unit> -p err   # Errors only
journalctl -b                 # Current boot only
journalctl --vacuum-size=500M # Free log disk space
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (28-Advanced-Storage-LVM-RAID-and-Filesystems) →](../28-Advanced-Storage-LVM-RAID-and-Filesystems/01-Storage-Architecture-Block-Devices-and-Partitions.md) |
