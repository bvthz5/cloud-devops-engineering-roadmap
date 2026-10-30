# 04 — Emergency Mode vs Rescue Mode

When systemd encounters fatal errors during startup, it halts into one of two diagnostic targets:

---

## 1. Comparison

| Feature | Rescue Mode (`rescue.target`) | Emergency Mode (`emergency.target`) |
| :--- | :--- | :--- |
| **Root Filesystem** | Mounted **Read-Write** (`rw`) | Mounted **Read-Only** (`ro`) |
| **Other Filesystems**| Local filesystems mounted | No additional filesystems mounted |
| **System Daemons** | Basic systemd services running | **No** daemons running (Bare kernel) |
| **Password Prompt** | Prompts for root password | Prompts for root password |
| **Use Case** | Routine maintenance, service fixing | Deep disk corruption repair, `fsck` |

---

## 2. Switching Targets at Runtime

```bash
# Switch to rescue mode from a live terminal
sudo systemctl isolate rescue.target

# Switch to emergency mode
sudo systemctl isolate emergency.target

# Resume normal boot
sudo systemctl default
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Initramfs and Dracut Internals](./03-Initramfs-and-Dracut-Internals.md) | [Index](../../../README.md) | [05 - Resetting Lost Root Password via GRUB →](./05-Resetting-Lost-Root-Password-via-GRUB.md) |
