# 01 - The Filesystem Root (`/`)

The **Root Directory (`/`)** is the absolute top-level starting point of the entire Linux directory hierarchy. 

---

## 🌲 Single Inverted Tree Model

Unlike Windows operating systems—which assign separate root letters to physical disk partitions (`C:\`, `D:\`, `E:\`)—Linux implements a **single unified tree structure**.

```text
Windows Model:
  C:\ (System Drive) ──> Program Files, Windows, Users
  D:\ (Data Drive)   ──> Movies, Backups

Linux Unified Model:
  / (Root Directory)
  ├── bin
  ├── etc
  ├── home
  ├── var
  └── mnt/
      └── data_drive (Mounted partition from second physical disk)
```

In Linux, every file, directory, hard drive, USB flash drive, network share, virtual process data, and hardware driver attaches somewhere under `/`.

---

## ⚙️ How Mounting Connects Storage to `/`

Physical storage devices (such as `/dev/sda1` or `/dev/nvme0n1p2`) are attached to specific directories in the tree hierarchy using a process called **Mounting**.

1. The Linux Kernel mounts the primary disk partition onto `/`.
2. Secondary drives or network volumes are mounted onto target directories (known as **Mount Points**), such as `/mnt/backup` or `/var/lib/mysql`.
3. To users and applications, the directory tree appears seamless. You navigate paths, not drive letters.

```bash
# Display mounted filesystems and their mount points under /
df -h
```

---

## 📌 Critical Properties of `/`

- **Absolute Paths Start Here:** Any path beginning with a forward slash (`/`) is an **Absolute Path** starting from the root directory (e.g., `/etc/nginx/nginx.conf`).
- **Owner & Permissions:** The root directory is owned by the `root` user (`root:root`) with standard permissions `755` (`rwxr-xr-x`).
- **Partition Isolation:** If `/` runs out of disk space (100% full), system services will fail to create temporary files or sockets, crashing critical OS processes.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - boot](./02-boot.md) |
