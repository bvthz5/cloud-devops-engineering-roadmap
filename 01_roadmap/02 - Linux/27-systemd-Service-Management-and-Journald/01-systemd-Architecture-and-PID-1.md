# 01 — systemd Architecture and PID 1

When the Linux kernel finishes booting, it mounts the root filesystem and executes `/sbin/init` as **Process ID 1** (PID 1). On modern Linux systems, `/sbin/init` is a symlink to `systemd`.

---

## 1. Why systemd Replaced SysVinit and Upstart

| Feature | Legacy SysVinit | Modern `systemd` |
| :--- | :--- | :--- |
| **Startup Model** | Sequential shell scripts (`/etc/init.d/`) | **Massively parallel** socket/D-Bus activated startup |
| **Process Tracking**| Brittle PID files (`/var/run/*.pid`) | Linux **Control Groups (cgroups)**: cannot escape via `fork()` |
| **Dependency Graph**| Hardcoded execution order (`S01`, `S02`) | Declarative dependency graph (`Wants=`, `Requires=`, `After=`) |
| **Logging** | Unstructured plaintext (`syslog`) | Structured, tamper-evident binary indexing (`journald`) |
| **Resource Control**| External `ulimit` or wrappers | Native kernel cgroups v2 (`MemoryMax=`, `CPUQuota=`) |

---

## 2. systemd Unit Types

Everything managed by `systemd` is a **Unit**, defined in declarative INI-style configuration files:

| Unit Extension | Unit Type | Purpose / Description |
| :--- | :--- | :--- |
| **`.service`** | Service | Background daemons, microservices, and scripts |
| **`.timer`** | Timer | Scheduled execution (replaces `cron`) |
| **`.mount`** | Mount point | Filesystem mount points (alternative to `/etc/fstab`) |
| **`.automount`** | Automount | Filesystems mounted on-demand when accessed |
| **`.socket`** | Socket | IPC or network sockets for on-demand lazy service activation |
| **`.target`** | Target | Synchronization group / grouping of units (replaces SysV runlevels) |
| **`.slice`** | Slice | Hierarchical resource management group for cgroups |
| **`.path`** | Path | Monitors file/directory changes (uses `inotify`) |

---

## 3. Targets vs Runlevels

| SysV Runlevel | systemd Target | Description |
| :--- | :--- | :--- |
| Runlevel 0 | `poweroff.target` | Shuts down and powers off the system |
| Runlevel 1 | `rescue.target` | Single-user administrative recovery mode |
| Runlevel 3 | `multi-user.target` | Normal multi-user command-line system (standard for cloud servers) |
| Runlevel 5 | `graphical.target` | Multi-user system with Graphical Desktop (X11/Wayland) |
| Runlevel 6 | `reboot.target` | Reboots the system |

```bash
# Check current default target
systemctl get-default

# Set server to headless CLI mode
sudo systemctl set-default multi-user.target
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (26-Linux-Firewalls-iptables-nftables-and-UFW)](../26-Linux-Firewalls-iptables-nftables-and-UFW/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Writing Custom systemd Service Units →](./02-Writing-Custom-systemd-Service-Units.md) |
