# 09 - Virtual Pseudo-Filesystems: `/proc`, `/sys`, and `/dev`

Linux features three special directories—`/proc`, `/sys`, and `/dev`—that consume **0 bytes of physical disk space**. They are **Virtual Pseudo-Filesystems** generated dynamically in RAM by the kernel to expose process state, hardware attributes, and device drivers as plain files.

---

## 🔬 1. The `/proc` Directory (procfs - Process & Kernel Information)

`/proc` exposes real-time kernel data structures, system statistics, hardware resource metrics, and running process information.

### Subdirectory Structure of `/proc`
- **Numeric Folders (`/proc/[PID]`):** Every running process gets a folder named after its Process ID (PID).
  - `/proc/1234/cmdline`: Command used to launch PID 1234.
  - `/proc/1234/environ`: Environment variables for PID 1234.
  - `/proc/1234/status`: Memory and state details for PID 1234.
- **System Information Files:**
  - `/proc/cpuinfo`: CPU model, core count, cache size, speed.
  - `/proc/meminfo`: RAM usage, buffers, cached memory, swap.
  - `/proc/version`: Kernel version and compiler build string.
  - `/proc/sys/`: Dynamically tune kernel behavior at runtime (`sysctl`).

```bash
# View RAM utilization directly from the kernel
cat /proc/meminfo

# Enable IP forwarding dynamically without rebooting
echo "1" | sudo tee /proc/sys/net/ipv4/ip_forward
```

---

## ⚙️ 2. The `/sys` Directory (sysfs - Hardware & Driver Topology)

Introduced in Linux Kernel 2.6, `/sys` organizes device drivers, hardware buses, power management states, and kernel subsystems into a unified object hierarchy.

### Common Paths in `/sys`
- `/sys/class/net/`: Network interface devices (`eth0`, `wlan0`).
- `/sys/block/`: Physical disk block devices (`sda`, `nvme0n1`).
- `/sys/devices/`: Full physical bus topology tree (PCIe, USB, ACPI).

---

## 🔌 3. The `/dev` Directory (devtmpfs - Device Nodes)

In Linux, hardware devices are accessed as files called **Device Nodes** inside `/dev`. Applications read and write to `/dev` nodes to interact with physical storage, serial ports, or terminal interfaces.

### Key Categories of Device Files
- **Block Devices (Data buffered in chunks):**
  - `/dev/sda`, `/dev/sdb`: SATA/SAS Hard Drives or SSDs.
  - `/dev/nvme0n1p1`: NVMe M.2 drive partition 1.
- **Character Devices (Unbuffered stream of characters):**
  - `/dev/tty1`, `/dev/pts/0`: Terminal displays and SSH sessions.
- **Special Kernel Pseudodevices:**
  - `/dev/null`: The "black hole" - discards all output written to it.
  - `/dev/zero`: Streams infinite null (zero) bytes (used to wipe disks or create blank files).
  - `/dev/urandom` / `/dev/random`: Streams cryptographically secure pseudo-random bytes.

```bash
# Create a blank 100MB file filled with zero-bytes
dd if=/dev/zero of=testfile.img bs=1M count=100
```

---

## ⬅️ Navigation
- Previous: [08 - `/tmp` and `/run`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/08-tmp-and-run.md)
- Next: [10 - `/mnt`, `/media`, and `/data`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/10-mnt-media-and-data.md)
