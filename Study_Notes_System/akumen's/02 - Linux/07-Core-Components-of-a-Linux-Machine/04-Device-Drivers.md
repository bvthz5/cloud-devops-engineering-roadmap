# 04 - Device Drivers & Loadable Kernel Modules (LKMs)

A **Device Driver** is a specialized software module running inside Kernel Space that translates generic OS read/write commands into physical electronic signaling understood by hardware controllers.

---

## 🚗 Categories of Linux Drivers

1. **Character Device Drivers (`char`):**
   - Process data as an unbuffered stream of individual bytes.
   - Examples: Serial ports (`/dev/ttyS0`), virtual consoles, `/dev/null`.
2. **Block Device Drivers (`block`):**
   - Process data in fixed-size blocks (e.g., 4096 bytes) with hardware caching.
   - Examples: SATA/NVMe drives (`/dev/sda`, `/dev/nvme0n1`), USB storage devices.
3. **Network Device Drivers (`net`):**
   - Handle packet transmission and reception across hardware network adapters (`eth0`, `wlan0`).

---

## 🧩 Loadable Kernel Modules (LKMs)

Monolithic kernel architectures (like Linux) would traditionally require recompiling the entire kernel binary whenever new hardware was added.

Linux solves this using **Loadable Kernel Modules (LKMs)**:
- Code modules (files ending in `.ko` under `/lib/modules/$(uname -r)/`) that can be loaded into or unloaded from memory dynamically without rebooting the system.

```bash
# List currently loaded kernel modules
lsmod | head -n 10

# Load a kernel module (e.g., WireGuard VPN driver)
sudo modprobe wireguard

# Unload a kernel module
sudo modprobe -r wireguard

# Inspect information about a specific kernel module
modinfo e1000e
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Linux Kernel Core](./03-Linux-Kernel-Core.md) | [README](./README.md) | [05 - System Libraries and Glibc](./05-System-Libraries-and-Glibc.md) |
