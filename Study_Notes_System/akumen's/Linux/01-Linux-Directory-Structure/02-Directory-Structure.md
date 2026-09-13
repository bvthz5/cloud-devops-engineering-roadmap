# 02 - Master Directory Structure Breakdown

The Linux filesystem organizes directories according to the **Filesystem Hierarchy Standard (FHS)**. Every directory serves a distinct functional purpose.

---

## 🏛️ Comprehensive Directory Master Table

| Directory | Full Name / Meaning | Primary Purpose | Real-World Examples Found Inside |
|---|---|---|---|
| **`/`** | **Root** | The base, parent, and starting point of the entire filesystem hierarchy. | All other directories branch from here. |
| **`/bin`** | **Essential User Binaries** | Fundamental command line executables needed in single-user mode and for all users. | `bash`, `cp`, `ls`, `mkdir`, `rm`, `cat` |
| **`/sbin`** | **System Binaries** | Essential maintenance and administrative commands, primarily run by `root`. | `iptables`, `fdisk`, `reboot`, `ip`, `fsck` |
| **`/boot`** | **Boot Loader Files** | Static files required to boot the Linux operating system. | GRUB configuration (`grub.cfg`), Kernel binary (`vmlinuz`), Initial RAM disk (`initramfs`) |
| **`/dev`** | **Device Files** | Virtual representation of hardware devices and kernel interfaces (managed by `udev`). | `/dev/sda` (disk), `/dev/null` (bit bucket), `/dev/urandom`, `/dev/tty` |
| **`/etc`** | **Host-Specific Configuration** | Plain text system configuration files and service scripts. No binaries allowed. | `/etc/passwd`, `/etc/fstab`, `/etc/hosts`, `/etc/nginx/nginx.conf`, `/etc/systemd/` |
| **`/home`** | **User Home Directories** | Personal storage for non-root users to store documents, configs, and personal scripts. | `/home/student`, `/home/devops`, `/home/ubuntu` |
| **`/root`** | **Root User Home** | The home directory for the administrative superuser `root`. Located directly on `/`. | `/root/.bashrc`, `/root/.ssh/authorized_keys` |
| **`/lib` & `/lib64`**| **Shared Libraries** | Essential shared libraries and kernel modules required by `/bin` and `/sbin` binaries. | `libc.so.6`, `/lib/modules/$(uname -r)/` |
| **`/usr`** | **User System Resources** | Secondary hierarchy containing read-only user utilities, documentation, and libraries. | `/usr/bin`, `/usr/share/doc`, `/usr/include` |
| **`/usr/local`** | **Local Hierarchy** | Software compiled and installed manually by the administrator (keeps `/usr` clean). | `/usr/local/bin`, `/usr/local/etc`, `/usr/local/go` |
| **`/opt`** | **Optional Add-on Packages** | Third-party, self-contained monolithic software applications. | `/opt/google/chrome`, `/opt/gitlab`, `/opt/aws` |
| **`/var`** | **Variable Data** | Files that change size dynamically while the OS is operating (logs, spool, caches). | `/var/log/syslog`, `/var/lib/docker`, `/var/spool/mail` |
| **`/tmp`** | **Temporary Files** | Scratchpad for temporary files created by applications. Often deleted on reboot. | Session files, temporary lockfiles, compile artifacts |
| **`/run`** | **Runtime State** | Volatile runtime data describing the running system since the last boot (RAM-backed). | PID files (`/run/sshd.pid`), control sockets |
| **`/proc`** | **Process & Kernel Virtual FS** | In-memory pseudo-filesystem exposing real-time process states and kernel tunable metrics. | `/proc/cpuinfo`, `/proc/meminfo`, `/proc/sys/net/` |
| **`/sys`** | **System & Device Subsystem FS** | In-memory pseudo-filesystem exporting device drivers, bus info, and power management. | `/sys/class/net/eth0/`, `/sys/block/` |
| **`/srv`** | **Service Data** | Site-specific data served by this system (web servers, FTP repositories). | `/srv/www/`, `/srv/ftp/` |
| **`/mnt`** | **Manual Mount Point** | Temporary mount point for system administrators to mount file systems manually. | `/mnt/backup-disk`, `/mnt/nfs-share` |
| **`/media`** | **Removable Media Mounts** | Automount target for user removable storage media (USB thumb drives, CD-ROMs). | `/media/student/MY_USB/` |

---

## 🧩 Categorizing the Directories

To memorize this effortlessly, group the directories into four functional quadrants:

```
┌──────────────────────────────────────┬──────────────────────────────────────┐
│       1. System Core & Config        │       2. Applications & Binaries     │
│                                      │                                      │
│ • /boot : Kernel & bootloader files  │ • /bin   : Everyday user commands    │
│ • /etc  : System-wide configs        │ • /sbin  : Admin system commands     │
│ • /lib  : Shared libraries           │ • /usr   : User programs & utilities │
│ • /root : Superuser home directory   │ • /opt   : 3rd party add-on apps     │
├──────────────────────────────────────┼──────────────────────────────────────┤
│       3. Kernel & Hardware State     │       4. Data, Logs & Ephemeral      │
│                                      │                                      │
│ • /dev  : Device files (disks, ttys) │ • /var   : Logs, database data, state│
│ • /proc : Live process & kernel info │ • /tmp   : Temporary files (scratch) │
│ • /sys  : Hardware & driver controls │ • /home  : Regular user workspaces   │
│ • /run  : Runtime PIDs and sockets   │ • /srv   : Hosted service assets     │
│                                      │ • /mnt   : Manual storage mounts     │
│                                      │ • /media : Plugged removable drives  │
└──────────────────────────────────────┴──────────────────────────────────────┘
```

---

## 🔍 Directory Details at a Glance

### Why `/boot` must be kept clean
The `/boot` directory houses kernel images (`vmlinuz-x.y.z`) and RAM disk images (`initramfs-x.y.z`). When operating system package managers update the Linux kernel, old kernels remain here unless pruned. If `/boot` hits 100% disk usage, subsequent system updates and security patches will fail!

### Why `/root` is NOT in `/home`
If `/home` is placed on a separate partition, network share, or encrypted disk volume that fails to mount during a boot disaster, the system administrator must still be able to log in to `/root` in single-user recovery mode to fix the issue. Keeping `/root` on the primary root filesystem ensures it is always accessible.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Filesystem Basics](./01-Filesystem-Basics.md) | [README](./README.md) | [03 - Paths and Navigation](./03-Paths-and-Navigation.md) |
