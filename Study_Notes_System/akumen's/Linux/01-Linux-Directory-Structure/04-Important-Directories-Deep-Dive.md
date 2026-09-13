# 04 - Important Directories Deep Dive

To be an effective Cloud & DevOps engineer, five directories require comprehensive understanding: `/etc`, `/var`, `/proc`, `/sys`, and `/dev`.

---

## 1. `/etc` — Host-Specific Configuration Hub

The `/etc` directory is the nervous system of a Linux host. It contains static text configuration files that control system and service behavior.

```
/etc/
├── passwd            # User account definitions (UID, GID, shell, home)
├── shadow            # Secure hashed passwords and expiration dates
├── group             # Group memberships
├── sudoers           # Privileged access configuration (visudo)
├── fstab             # Static filesystem mount table
├── hosts             # Static IP-to-hostname mappings
├── resolv.conf       # DNS nameserver configuration
├── os-release        # Linux distribution identification
├── sysctl.conf       # Kernel runtime parameter defaults
├── crontab           # System-wide scheduled task definitions
└── systemd/
    └── system/       # Custom and override service unit files
```

### Critical Files to Know:
1. **`/etc/passwd`:** Read by all applications to map UIDs to human names. 
   - Format: `username:x:UID:GID:Comment:HomeDir:Shell`
2. **`/etc/fstab`:** Defines what drives to mount at boot. A syntax error here can cause a server to drop into emergency recovery mode.
3. **`/etc/resolv.conf`:** Specifies DNS servers (`nameserver 8.8.8.8`). Often managed dynamically by `systemd-resolved` or NetworkManager.
4. **`/etc/hosts`:** Local override for DNS resolution. Checked before querying network DNS (governed by `/etc/nsswitch.conf`).

---

## 2. `/var` — Variable State, Storage & Logs

Unlike `/etc` (which changes only when configurations are edited), `/var` is designed for data that continuously grows and mutates during normal system operation.

```
/var/
├── log/              # Centralized system and service log files
│   ├── syslog        # Ubuntu/Debian general system log
│   ├── messages      # RHEL/CentOS general system log
│   ├── auth.log      # SSH and privilege escalation logs
│   ├── journal/      # systemd binary journal logs
│   └── nginx/        # Nginx access and error logs
├── lib/              # Dynamic application state and packages
│   ├── docker/       # Container images, layers, and volumes!
│   ├── apt/ or rpm/  # Package manager databases
│   └── mysql/        # Default database storage directory
├── cache/            # Temporary application cache (e.g. package lists)
├── spool/            # Queued jobs (mail queues, print spools, at/cron)
└── tmp/              # Temporary files preserved across reboots!
```

> [!TIP]
> **DevOps Notice:** In containerized environments, `/var/lib/docker` or `/var/lib/containerd` is frequently mounted on a dedicated, high-speed disk volume to prevent image pulls from filling up the root OS drive.

---

## 3. `/proc` — Virtual Process & Kernel State Filesystem

`/proc` is a **pseudo-filesystem (procfs)**. It does not exist on any physical SSD or hard drive. It is generated on-the-fly by the Linux kernel directly in RAM.

If you run `ls -lh /proc/cpuinfo`, you will see a size of **0 bytes**, yet running `cat /proc/cpuinfo` produces pages of processor details!

### Key System Information Files:
- `/proc/cpuinfo`: CPU model, core count, cache sizes, flags (e.g., virtualization support `vmx`/`svm`).
- `/proc/meminfo`: Real-time RAM statistics, buffer usage, swap usage, and cached memory.
- `/proc/loadavg`: 1, 5, and 15-minute system load averages.
- `/proc/uptime`: Total seconds system has been up and idle.
- `/proc/sys/`: Live kernel tunable parameters! Modifiable at runtime:
  ```bash
  # Enable IP forwarding (routing between interfaces):
  echo 1 > /proc/sys/net/ipv4/ip_forward
  ```

### Inspecting Running Processes via `/proc/[PID]/`
Every active process has a numeric directory inside `/proc` corresponding to its Process ID (PID):

```bash
$ ls /proc/1234/
cmdline    # The exact command line used to launch the process
cwd        # Symlink to the process's current working directory
environ    # Process environment variables (null-byte separated)
exe        # Symlink to the actual binary file on disk
fd/        # Directory containing all open file descriptors
status     # Detailed human-readable process status & memory usage
```

```bash
# Example: Inspect what binary PID 1234 is running
$ ls -l /proc/1234/exe
lrwxrwxrwx 1 root root 0 Sep 13 11:00 /proc/1234/exe -> /usr/sbin/nginx

# Example: Inspect open files/sockets for PID 1234
$ ls -l /proc/1234/fd/
```

---

## 4. `/sys` — System & Hardware Device Subsystem (sysfs)

Like `/proc`, `/sys` is an in-memory virtual filesystem (**sysfs**). While `/proc` focuses primarily on processes and general kernel parameters, `/sys` provides a strictly structured, object-oriented view of **physical hardware, device drivers, power states, and kernel subsystems**.

```
/sys/
├── block/          # Block storage devices (sda, nvme0n1)
├── bus/            # Hardware buses (pci, usb, i2c)
├── class/          # Device classes
│   └── net/        # Network interfaces (eth0, wlan0, docker0)
└── power/          # System power states (suspend, hibernate)
```

```bash
# Check link carrier state of eth0 directly from the kernel:
$ cat /sys/class/net/eth0/carrier
1  # (1 = cable connected / link up, 0 = link down)

# Check operational state:
$ cat /sys/class/net/eth0/operstate
up
```

---

## 5. `/dev` — Hardware & Pseudo Device Nodes

`/dev` houses device nodes managed by the **udev** device manager daemon. These nodes allow programs to speak to drivers using standard file operations.

### Device Numbering: Major and Minor Numbers
```bash
$ ls -l /dev/sda1
brw-rw---- 1 root disk 8, 1 Sep 13 09:00 /dev/sda1
```
- **Major Number (`8`):** Identifies the device driver responsible (8 = SCSI/SATA disk driver).
- **Minor Number (`1`):** Identifies the specific partition or device handled by that driver (1 = partition 1).

### The Famous Pseudo-Devices:
1. **`/dev/null` (The Bit Bucket):** Discards all data written to it; returns EOF on read.
   ```bash
   # Suppress stderr:
   command 2> /dev/null
   ```
2. **`/dev/zero`:** Yields an infinite stream of null bytes (`\0`). Used for zero-wiping disks or creating empty files:
   ```bash
   dd if=/dev/zero of=testfile bs=1M count=100
   ```
3. **`/dev/urandom`:** Cryptographically secure pseudo-random number generator (CSPRNG).
4. **`/dev/shm` (Shared Memory):** A RAM-backed `tmpfs` filesystem accessible for high-speed inter-process communication. Writing here writes directly into RAM.
