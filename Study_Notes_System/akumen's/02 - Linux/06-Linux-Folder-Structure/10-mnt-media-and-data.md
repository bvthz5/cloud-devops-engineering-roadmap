# 10 - Storage Mount Points: `/mnt`, `/media`, and `/data`

When attaching physical drives, network shares, USB sticks, or cloud block storage volumes to Linux, mount points are designated under specific directories according to usage context.

---

## 🛠️ 1. The `/mnt` Directory (Manual / Temporary Mounts)

`/mnt` (short for **Mount**) is designated for system administrators to mount filesystems temporarily while carrying out administrative tasks or system recovery.

### Use Cases for `/mnt`
- Mounting a corrupted disk partition from a live CD to perform repair.
- Mounting a temporary NFS network backup share to transfer database dumps.
- Subdirectories created under `/mnt` (e.g., `/mnt/backup`, `/mnt/ext_disk`).

```bash
# Mount a partition manually to /mnt/disk2
sudo mkdir -p /mnt/disk2
sudo mount /dev/sdb1 /mnt/disk2
```

---

## 💾 2. The `/media` Directory (Removable Media Automounting)

`/media` is reserved for automounting removable media inserted into the workstation or server.

### Standard Subdirectories in `/media`
When a user plugs in a USB flash drive or inserts an optical disc on modern desktop environments (Ubuntu Desktop, Fedora, RHEL):
- `/media/username/USB_STICK/`
- `/media/cdrom/` or `/media/floppy/`

The operating system's desktop daemon (like UDisks2) automatically creates subdirectories under `/media/$USER/` and unmounts them upon safe removal.

---

## 🗄️ 3. The `/data` Directory (Custom Enterprise Mount Point)

It is common in production enterprise servers and cloud infrastructure to see top-level directories such as **`/data`**, **`/data1`**, **`/app`**, or **`/u01`**.

### Important Distinction: Custom vs Mandatory FHS Directories
- **Not in FHS Standard:** `/data` is **NOT** a standard mandatory directory defined by the official Linux Filesystem Hierarchy Standard (FHS).
- **Enterprise Best Practice:** System architects create `/data` as a top-level mount point for separate high-capacity SAN storage arrays, EBS volumes, or database data stores (e.g., `/data/postgres`, `/data/kafka_logs`).

```bash
# Mount an AWS EBS Volume to a custom /data partition
sudo mkdir -p /data
sudo mount /dev/nvme1n1 /data
```

---

## ⚖️ Summary Comparison Matrix

| Directory | FHS Standard? | Primary Mount Purpose | Automounted by OS? |
| :--- | :---: | :--- | :---: |
| **`/mnt`** | Yes | Temporary manual admin mounts | No |
| **`/media`** | Yes | Removable media (USB, CD-ROM) | Yes (Desktop DE) |
| **`/data`** | No (Custom) | Dedicated high-capacity application/DB storage | No (Configured in `/etc/fstab`) |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - proc sys and dev](./09-proc-sys-and-dev.md) | [README](./README.md) | [11 - Symbolic Links bin sbin lib](./11-Symbolic-Links-bin-sbin-lib.md) |
