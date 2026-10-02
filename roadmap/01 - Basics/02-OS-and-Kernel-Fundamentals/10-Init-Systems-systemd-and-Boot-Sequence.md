# 10 — Init Systems, systemd Architecture, and OS Boot Sequence

---

## 1. The Complete Linux OS Boot Process

Understanding how Linux boots from cold silicon to an operational multi-user environment is essential for troubleshooting broken kernels, corrupt storage, and boot loops.

```text
+-------------------------------------------------------------+
|                     Linux Boot Pipeline                     |
+-------------------------------------------------------------+
| Stage 1: Power-On & Firmware (UEFI / BIOS)                 |
|   - Hardware POST check                                     |
|   - Initializes DRAM, PCIe bus, and storage devices         |
|   - Reads EFI System Partition (ESP) on GPT disk            |
+-------------------------------------------------------------+
                              │
                              ▼
| Stage 2: Bootloader (GRUB2 / systemd-boot)                 |
|   - Presents kernel boot menu (interactive selection)       |
|   - Loads Linux Kernel (`vmlinuz`) and Initramfs into RAM   |
|   - Passes kernel command-line parameters (`root=UUID=...`) |
|   - Jumps to kernel entry point in RAM                      |
+-------------------------------------------------------------+
                              │
                              ▼
| Stage 3: Kernel Initialization                              |
|   - Decompresses and initializes CPU registers, MMU, caches |
|   - Mounts `initramfs` (Initial RAM File System) in memory  |
|   - Executes `/init` script inside initramfs                |
|   - Loads essential storage drivers (NVMe, LVM, ext4, RAID) |
+-------------------------------------------------------------+
                              │
                              ▼
| Stage 4: Root Filesystem Pivot & Switch                     |
|   - Detects real physical disk partition via UUID           |
|   - Runs filesystem integrity check (`fsck`)                |
|   - Mounts real root filesystem (`/`) read-only, then rw    |
|   - Executes `pivot_root` or `switch_root`                  |
+-------------------------------------------------------------+
                              │
                              ▼
| Stage 5: Init System Execution (PID 1)                     |
|   - Kernel spawns the first user-space process: `/sbin/init`|
|   - On modern Linux, `/sbin/init` is symlinked to `systemd` |
|   - systemd loads default target (`multi-user.target`)      |
|   - Mounts virtual filesystems (`/proc`, `/sys`, `/dev`)    |
|   - Spawns background daemons, network, and SSH server      |
+-------------------------------------------------------------+
```

---

## 2. The Evolution of Init Systems

The init system is the mother of all processes (PID 1). It initializes user space, starts system services, and reaps orphaned child processes.

```text
SysVinit (1980s - 2010)       Upstart (2006 - 2014)         systemd (2010 - Present)
- Sequential bash scripts     - Event-driven (Ubuntu)       - Declarative unit files
- Slow sequential boot        - Complex state transitions   - Parallel dependency startup
- Runlevels (0, 1, 3, 5, 6)   - Replaced by systemd         - Cgroup process tracking
- Poor crash recovery                                       - Integrated journald logging
```

---

## 3. systemd Architecture & Concepts

**`systemd`** is a comprehensive system and service manager that has become the standard across all major enterprise distributions (Ubuntu, Debian, RHEL, CentOS, Rocky Linux, Arch).

### Core Features of systemd
1. **Parallel Execution:** Uses socket and D-Bus activation to start services concurrently, drastically reducing boot times.
2. **cgroup Process Tracking:** Tracks processes using Linux control groups rather than fragile PID files. When a service is stopped, systemd kills all worker processes and child threads reliably without leaving behind orphaned background daemons.
3. **Declarative Unit Files:** Replaces messy shell scripts with standardized, declarative INI-style configuration files.

---

## 4. systemd Unit Types

Everything managed by systemd is known as a **Unit**, represented by a specific file extension:

| Unit Type | Extension | Purpose & Example |
| :--- | :--- | :--- |
| **Service** | `.service` | Manages daemons and applications (e.g., `nginx.service`, `docker.service`). |
| **Target** | `.target` | Groups units into synchronization boot points (e.g., `multi-user.target`). |
| **Timer** | `.timer` | Cron alternative for scheduled jobs with microsecond precision (`backup.timer`). |
| **Socket** | `.socket` | IPC or network socket activation; starts service on first connection (`sshd.socket`). |
| **Mount** | `.mount` | Controls filesystem mount points (auto-generated from `/etc/fstab`). |
| **Path** | `.path` | Activates services when a file or directory is modified (`inotify`). |

### systemd Targets (Modern Runlevels)
- **`poweroff.target`** (Old Runlevel 0): Halts and shuts down the computer.
- **`rescue.target`** (Old Runlevel 1): Single-user maintenance mode (no networking).
- **`multi-user.target`** (Old Runlevel 3): Standard multi-user text console environment with networking (standard for production cloud servers).
- **`graphical.target`** (Old Runlevel 5): Full GUI desktop environment.
- **`reboot.target`** (Old Runlevel 6): Reboots the machine.

---

## 5. Writing a Production-Grade systemd Service Unit

Below is an enterprise-ready systemd service file demonstrating best practices: sandboxing, automated restarts, logging, and security boundaries.

Save to `/etc/systemd/system/payments-api.service`:

```ini
[Unit]
Description=Payments Processing API Service
Documentation=https://docs.internal.org/payments
After=network-online.target postgresql.service
Wants=network-online.target

[Service]
Type=simple
User=appuser
Group=appuser
WorkingDirectory=/opt/payments-api

# Binary execution
ExecStart=/opt/payments-api/bin/server --config /etc/payments/config.yaml
ExecReload=/bin/kill -HUP $MAINPID

# Process Lifecycle & Crash Recovery
Restart=always
RestartSec=5s
TimeoutStopSec=30s

# Resource Constraints (cgroups)
CPUQuota=200%
MemoryMax=2G
LimitNOFILE=65536

# Security Hardening & Sandboxing
NoNewPrivileges=true
ProtectSystem=strict
ProtectHome=true
PrivateTmp=true
ProtectKernelTunables=true
ProtectControlGroups=true
ReadOnlyPaths=/opt/payments-api
ReadWritePaths=/var/log/payments

# Environment Configuration
Environment=ENV=production PORT=8080
EnvironmentFile=-/etc/default/payments-api

[Install]
WantedBy=multi-user.target
```

---

## 6. Essential `systemctl` & `journalctl` Commands

```bash
# 1. Reload systemd manager configuration after editing unit files
sudo systemctl daemon-reload

# 2. Enable and immediately start a service
sudo systemctl enable --now payments-api.service

# 3. Check detailed service health, active status, and recent logs
systemctl status payments-api.service

# 4. View real-time logs streamed from stdout/stderr of a service
journalctl -u payments-api.service -f

# 5. Filter logs by time and error priority (errors only)
journalctl -u payments-api.service --since "1 hour ago" -p err

# 6. Check why a system boot was slow (profile boot bottleneck)
systemd-analyze blame
systemd-analyze critical-chain
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Kernel Networking Sockets and Security](./09-Kernel-Networking-Sockets-and-Security.md) | [Index](../../../README.md) | [11 - Kernel Parameters Sysctl and Kernel Logs →](./11-Kernel-Parameters-Sysctl-and-Kernel-Logs.md) |
