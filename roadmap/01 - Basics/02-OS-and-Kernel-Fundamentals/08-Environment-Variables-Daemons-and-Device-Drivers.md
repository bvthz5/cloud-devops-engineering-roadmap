# 08 — Environment Variables, Shells, Daemons, and Kernel Drivers

---

## 1. Environment Variables

**Environment Variables** are dynamic, key-value pairs stored in a process's memory space that configure runtime behavior, system paths, and application settings without requiring code modification.

### How Environment Variables Work in Linux
- When a process calls `fork()`, the child inherits an exact copy of the parent's environment variable block.
- If a child modifies or sets a variable, that change is strictly local to the child; **a child process can never alter its parent's environment**.
- In C/glibc, environment variables reside in a null-terminated array of strings pointed to by the global pointer `char **environ`.

```text
Parent Process (Bash Shell)
  ENV: { PORT=8080, DB_HOST=postgres.local }
       │
       ├─ Calls fork()
       ▼
Child Process (Inherits exact copy)
  ENV: { PORT=8080, DB_HOST=postgres.local }
  Sets ENV: PORT=9000
  (Parent's PORT remains 8080 unaffected)
```

### Inspecting a Live Process's Environment
Every running process exposes its environment variables through the kernel's procfs interface:
```bash
# View environment variables of PID 1240 (null-byte separated)
sudo cat /proc/1240/environ | tr '\0' '\n'
```

### Twelve-Factor App Methodology
In modern cloud and container engineering, the **Third Factor** of the 12-Factor App methodology mandates storing application config strictly in environment variables (e.g., database credentials, API endpoints, log levels).

---

## 2. Shells: The User-to-Kernel Interface

A **Shell** is a command-line interpreter that reads commands typed by the user or read from scripts and executes system calls to carry them out.

```text
User Input ──► [ Shell (Bash / Zsh) ] ──► System Calls (fork, exec, pipe) ──► [ Linux Kernel ]
```

### Shell Types & Modes
1. **Interactive Login Shell:** Started when logging in via SSH or virtual console. Reads `/etc/profile`, then `~/.bash_profile` or `~/.profile`.
2. **Interactive Non-Login Shell:** Started by opening a new terminal window inside an existing session. Reads `~/.bashrc`.
3. **Non-Interactive Shell:** Used to execute automated shell scripts (`#!/bin/bash`). Reads `$BASH_ENV`.

---

## 3. Daemons: True Background Processes

A **Daemon** is a long-running background process that runs detached from any controlling terminal (`tty`), typically starting at system boot and waiting to handle requests or periodic events (e.g., `sshd`, `cron`, `nginx`).

### Traditional Unix "Double-Fork" Daemonization
Historically, to turn a process into a well-behaved background daemon, a program had to execute a strict ritual:

```text
1. fork() and exit parent
   Returns control to the shell prompt immediately.

2. setsid() (Create new Session)
   Detaches the process from the controlling terminal (no SIGHUP when terminal closes).

3. fork() a second time and exit parent
   Guarantees the daemon can never accidentally re-acquire a controlling terminal.

4. chdir("/")
   Changes working directory to root so the daemon does not lock mounted filesystems.

5. umask(0)
   Resets file creation mask to ensure predictable file permissions.

6. Close file descriptors (0, 1, 2)
   Redirects stdin, stdout, stderr to /dev/null to prevent terminal deadlocks.
```

### Modern Daemonization with `systemd`
In modern Linux with systemd, applications **do not need to daemonize themselves**. Writing background services is drastically simplified:
- The service runs as a simple foreground process.
- systemd handles detaching, sandboxing, stdout/stderr logging (routed automatically to `journald`), process supervision, and automatic restarts.

---

## 4. Services vs Daemons

- **Daemon:** The raw executable binary designed to run continuously in the background (e.g., `/usr/sbin/sshd`).
- **Service:** The managed operational abstraction containing configuration, lifecycle rules, dependencies, and health checks (e.g., `sshd.service` managed via `systemctl`).

---

## 5. Device Drivers

A **Device Driver** is privileged kernel code that translates generic operating system I/O requests into the low-level, proprietary electrical signals and command registers understood by a specific hardware peripheral.

```text
User Application: write(fd, buffer, 4096)
       │
       ▼ VFS Layer
Generic Block Layer
       │
       ▼ Device Driver (e.g., nvme.ko)
Registers & Commands (PCIe Doorbell Registers)
       │
       ▼ Hardware
Physical NVMe Controller & NAND Flash Chips
```

### Device Categories
1. **Character Devices:** Stream data sequentially byte-by-byte without buffering (e.g., `/dev/tty`, keyboards, sound cards).
2. **Block Devices:** Transfer data in fixed-size blocks (e.g., 512 bytes or 4 KB) with caching and random-access seeking (e.g., `/dev/sda`, `/dev/nvme0n1`).
3. **Network Devices:** Interface with network cards transmitting packets (`eth0`, `ens5`). They do not appear as file nodes in `/dev`, but are manipulated via sockets and `ip` / `ifconfig`.

---

## 6. Loadable Kernel Modules (LKM)

The Linux kernel is monolithic, but it is **dynamically modular**. A **Loadable Kernel Module (LKM)** is an object file containing code that can be loaded into and unloaded from the running kernel on demand, **without rebooting the operating system**.

### Common Uses of Kernel Modules
- Device drivers for newly attached hardware (USB, PCIe NIC).
- Filesystem drivers (e.g., loading `zfs.ko` or `wireguard.ko`).
- Security extensions and packet filtering engines (`iptable_filter`, `nf_conntrack`).

### Managing Kernel Modules via CLI
```bash
# 1. List all currently loaded kernel modules
lsmod | head -n 15

# 2. View metadata and parameters of a specific module
modinfo overlay

# 3. Load a module along with all its dependencies
sudo modprobe overlay

# 4. Unload a module safely
sudo modprobe -r overlay

# 5. Low-level direct insertion (does not resolve dependencies)
sudo insmod /path/to/my_driver.ko

# 6. Low-level direct removal
sudo rmmod my_driver
```

### DKMS (Dynamic Kernel Module Support)
When the Linux kernel is upgraded via `apt` or `dnf`, proprietary out-of-tree modules (such as Nvidia GPU drivers or ZFS storage modules) must be recompiled for the new kernel version. **DKMS** automatically recompiles and installs these kernel modules during the system update process.
