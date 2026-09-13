# 05 - Filesystem Hierarchy Standard (FHS) & Modern Linux Evolution

---

## 1. What is the Filesystem Hierarchy Standard (FHS)?

The **Filesystem Hierarchy Standard (FHS)** is an open specification maintained by the Linux Foundation. It establishes consistent directory locations so that:
- Software developers know where to install program binaries, configuration files, and documentation.
- System administrators can manage backups, partitioning, and security predictably across different Linux distributions (Ubuntu, Debian, RHEL, Alpine, Arch).

### The Core FHS 2x2 Matrix

The FHS categorizes all files into a matrix based on two fundamental criteria:
1. **Sharable vs. Unsharable:** Can this file be shared over a network (e.g., via NFS) with other machines?
2. **Static vs. Variable:** Does the file remain unchanged unless explicitly upgraded/configured, or does it change continuously at runtime?

```
                    ┌─────────────────────────┬─────────────────────────┐
                    │        Sharable         │       Unsharable        │
┌───────────────────┼─────────────────────────┼─────────────────────────┤
│                   │ /usr (User binaries)    │ /etc (Host configs)     │
│      Static       │ /opt (3rd party apps)   │ /boot (Host kernel)     │
│                   │ /usr/share (Docs/man)   │                         │
├───────────────────┼─────────────────────────┼─────────────────────────┤
│                   │ /var/mail (Mailboxes)   │ /var/log (Host logs)    │
│     Variable      │ /var/spool/news         │ /var/run -> /run (PIDs) │
│                   │                         │ /tmp (Host temp files)  │
└───────────────────┴─────────────────────────┴─────────────────────────┘
```

---

## 2. The UsrMerge Evolution: Why `/bin` points to `/usr/bin`

If you open a modern Linux system (Ubuntu 20.04+, RHEL 8/9, Debian 11+, Arch) and run `ls -ld /bin /sbin /lib /lib64`, you will notice they are symbolic links:

```bash
$ ls -ld /bin /sbin /lib /lib64
lrwxrwxrwx 1 root root 7 May 10 12:00 /bin -> usr/bin
lrwxrwxrwx 1 root root 8 May 10 12:00 /sbin -> usr/sbin
lrwxrwxrwx 1 root root 7 May 10 12:00 /lib -> usr/lib
lrwxrwxrwx 1 root root 9 May 10 12:00 /lib64 -> usr/lib64
```

### The 1970s History: Why were they split originally?
In 1971, Ken Thompson and Dennis Ritchie ran Unix on a DEC PDP-11. Their primary storage drive was an **RK05 disk pack with only 1.5 megabytes of capacity**.
When the root disk ran out of storage, they mounted a second RK05 drive to `/usr` and moved user executables and libraries there. Commands critical to boot before mounting disks stayed in `/bin`, while non-essential binaries moved to `/usr/bin`.

### Why Modern Linux Merged Them (The "UsrMerge"):
1. **Initramfs eliminated the need:** Modern Linux boots using an initial RAM disk (`initramfs`), which loads all storage drivers before mounting the real root filesystem.
2. **Atomic Upgrades & OS Snapshots:** The entire operating system binary footprint can now be packaged into a read-only, verifiable `/usr` partition (e.g., in immutable operating systems like Flatcar, Fedora Silverblue, Talos).
3. **Eliminates Arbitrary Splitting:** Deciding whether a tool like `ip` belonged in `/bin` or `/sbin` or `/usr/bin` was an ongoing source of bugs. Merging provides a single canonical location.

---

## 3. The `/run` Directory Revolution

In older Linux distributions, runtime PID files and sockets were scattered across `/var/run` and `/var/lock`. 
The problem? `/var` might be on a separate physical partition that is mounted relatively late in the boot process. But early-boot daemons (like `systemd` or `udev`) need to record process IDs and sockets immediately.

**Solution:** Modern Linux mounts a `tmpfs` (RAM filesystem) directly to `/run` at the earliest stage of boot.
`/var/run` is now simply a backward-compatible symlink:
```bash
$ ls -ld /var/run
lrwxrwxrwx 1 root root 6 Jan  1 00:00 /var/run -> ../run
```

---

## 4. Frequently Confusing Distinctions Explained

### 1. `/` vs. `/root`
- **`/` (Root Directory):** The top-level parent of the entire filesystem tree. Every file and directory is a descendant of `/`.
- **`/root` (Root User's Home Directory):** The private home folder for the superuser `root`. Analogous to `/home/john`, but kept directly on the root partition so `root` can always log in even if `/home` fails to mount.

### 2. `/bin` vs. `/sbin`
- **`/bin`:** Essential user binaries runnable by everyone (`ls`, `cat`, `grep`).
- **`/sbin` (System Binaries):** Maintenance, partitioning, firewall, and system administration commands intended for `root` or `sudo` (`fdisk`, `iptables`, `reboot`, `visudo`).

### 3. `/etc` vs. `/var`
- **`/etc`:** Contains static, declarative configuration files created or edited by admins. Programs should never write ongoing dynamic binary state here.
- **`/var`:** Contains dynamic, continuously fluctuating runtime data (application databases, logs, spool files).

### 4. `/tmp` vs. `/var/tmp`
- **`/tmp`:** Temporary scratch space. On modern systems, it is mounted as a `tmpfs` (in RAM) and wiped on every reboot. Files are often automatically deleted after 10 days of non-use.
- **`/var/tmp`:** Persistent temporary files. It resides on disk and survives system reboots. Intended for large temporary files (like multi-gigabyte ISO images or compiler caches) that must survive a crash or reboot.

### 5. `/opt` vs. `/usr/local`
- **`/usr/local`:** Used when compiling software manually from source (`make && sudo make install`). It mimics the standard Linux tree layout (`/usr/local/bin`, `/usr/local/lib`, `/usr/local/share`).
- **`/opt` (Optional):** Used for self-contained, monolithic third-party packages that keep everything in a single directory bundle (e.g., `/opt/google/chrome` contains its own binaries, libraries, and resources in one folder).

### 6. `/mnt` vs. `/media`
- **`/mnt`:** Reserved for the system administrator to manually mount temporary filesystems or recovery images (`mount /dev/sdb1 /mnt`).
- **`/media`:** Used by desktop environments and automounting daemons to dynamically mount removable hardware devices (USB flash drives, SD cards, external HDDs).
