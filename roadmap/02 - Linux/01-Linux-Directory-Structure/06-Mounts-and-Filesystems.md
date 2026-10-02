# 06 - Mounts, Partitions & Filesystem Mechanics

In Linux, storage devices do not have drive letters. To access data on a disk, partition, or network share, the storage volume must be attached to an existing folder in the directory tree. This process is called **mounting**.

---

## 1. The Concept of Mounting

A **mount point** is any directory on the Linux filesystem used as the doorway to access a mounted partition.

```
Before Mount:
  / (Root Disk Partition 1)
  └── mnt/
      └── data/ (empty directory)

Action:
  mount /dev/sdb1 /mnt/data

After Mount:
  / (Root Disk Partition 1)
  └── mnt/
      └── data/ ───> Reading/Writing directly to [Disk 2 Partition 1 (/dev/sdb1)]
```

> [!WARNING]
> **Mount Shadowing Warning:**
> If `/mnt/data` already contained files *before* you mounted `/dev/sdb1` to it, those original files are **not deleted**, but they become **invisible and inaccessible** until the filesystem is unmounted!

---

## 2. The `mount` and `umount` Commands

### Mounting a Partition:
```bash
# Syntax: mount [options] <device> <mount_point>
$ sudo mount /dev/nvme1n1p1 /data

# Mount an ISO disk image:
$ sudo mount -o loop ubuntu-22.04.iso /mnt/iso

# Mount a read-only filesystem:
$ sudo mount -o ro /dev/sdb1 /mnt/backup
```

### Unmounting a Filesystem:
```bash
# You can unmount by device or by mount point:
$ sudo umount /data
# OR
$ sudo umount /dev/nvme1n1p1
```

### Handling "Target is busy" (`umount: target is busy`):
If a user shell or background process has an open file inside the mount point, `umount` will fail.
```bash
# Find which process is locking the mount point:
$ sudo lsof +f -- /data
# OR
$ sudo fuser -vm /data

# Kill offending processes (or gracefully close them)
$ sudo fuser -k -m /data
$ sudo umount /data

# Emergency lazy unmount (detaches immediately, cleans up when files close):
$ sudo umount -l /data
```

---

## 3. Persistent Mounts: `/etc/fstab` (File Systems Table)

Commands run with `mount` are temporary and vanish on reboot. To make mounts persistent across reboots, define them in `/etc/fstab`.

### The 6 Columns of `/etc/fstab`:
```
# <file system>                            <mount point>  <type>  <options>       <dump>  <pass>
UUID=3a8e9d8f-7c6b-4e3a-9e1d-8f2c7b5a4d3e  /              ext4    defaults        0       1
UUID=4b9f0e1a-8d7c-5f4b-0f2e-9a3d8c6b5e4f  /data          xfs     defaults,noatime 0       2
/dev/sdb1                                  /mnt/storage   ext4    defaults,nofail 0       2
```

| Column | Field Name | Description & Best Practices |
|:---:|---|---|
| **1** | **File System** | The device identifier. **Always use `UUID=`**, never `/dev/sda1`, because drive letters can shift if disks are added or controllers reorder! |
| **2** | **Mount Point** | The directory path where the filesystem will be attached (must exist). |
| **3** | **Type** | Filesystem type (`ext4`, `xfs`, `btrfs`, `nfs`, `cifs`, `vfat`). |
| **4** | **Options** | Comma-separated mount options (`defaults`, `ro`, `rw`, `noexec`, `nosuid`, `noatime`, `nofail`). |
| **5** | **Dump** | Used by the legacy `dump` backup utility. Almost always set to `0` (disabled). |
| **6** | **Pass (fsck order)**| Filesystem check priority at boot: `1` for root `/`, `2` for other partitions, `0` to skip fsck (e.g., XFS or swap). |

> [!IMPORTANT]
> **The `nofail` Option for Cloud & DevOps:**
> In AWS EC2 or Kubernetes nodes with external EBS/SAN volumes attached, always add the `nofail` option (e.g. `defaults,nofail`).
> Without `nofail`, if an external volume fails to attach or is delayed, **Linux will halt booting and drop into emergency mode**, taking your production server offline!

### How to Find the UUID of a Storage Device:
```bash
$ sudo blkid
/dev/sda1: UUID="9e2f4b1a-..." TYPE="ext4" PARTUUID="..."

# Or use lsblk:
$ lsblk -f
```

### The Golden Rule Before Rebooting:
Never reboot immediately after editing `/etc/fstab`. Test it with `mount -a`:
```bash
$ sudo mount -a
```
If `mount -a` prints any syntax error, fix it immediately! If it prints nothing, your `/etc/fstab` is valid and safe.

---

## 4. Bind Mounts (`mount --bind`)

A **bind mount** creates an alias or view of a directory somewhere else in the directory tree without creating symbolic links.

```bash
$ sudo mount --bind /var/log /mnt/logs_mirror
```
Bind mounts are fundamental to **Docker container volumes**. When you run:
```bash
docker run -v /home/user/app:/app myimage
```
The container runtime uses Linux kernel bind mounting into the container's mount namespace!

---

## 5. Remounting Live Filesystems

If a disk experiences I/O errors, the kernel often remounts the root filesystem as **read-only (`ro`)** to prevent data corruption.
To remount it back to read-write mode after remediation:
```bash
$ sudo mount -o remount,rw /
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - FHS and Modern Linux](./05-FHS-and-Modern-Linux.md) | [README](./README.md) | [07 - Practical Commands](./07-Practical-Commands.md) |
