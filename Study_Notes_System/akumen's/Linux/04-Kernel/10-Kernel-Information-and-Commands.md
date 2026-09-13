# 10 - Kernel Information & Administration Commands

System administrators, DevOps engineers, and SREs use a specialized set of command-line tools to query kernel state, inspect ring buffer logs, and tune live kernel variables without rebooting.

---

## 1. Kernel Identification & Release Info

```bash
# 1. Print full kernel system information:
$ uname -a
Linux prod-k8s-node01 6.5.0-35-generic #35-Ubuntu SMP PREEMPT_DYNAMIC x86_64 GNU/Linux

# 2. Print exact kernel release version:
$ uname -r
6.5.0-35-generic

# 3. Print hardware CPU architecture:
$ uname -m
x86_64

# 4. View kernel compile-time parameters and configuration:
$ zcat /proc/config.gz | grep CONFIG_NAMESPACES
CONFIG_NAMESPACES=y
```

---

## 2. Kernel Ring Buffer Messages: `dmesg`

When the Linux kernel boots or encounters hardware interrupts, storage faults, OOM events, and driver errors, it logs messages to an in-memory circular buffer called the **Kernel Ring Buffer**.

```bash
# 1. View all kernel messages with human-readable timestamps:
$ sudo dmesg -T

# 2. Filter exclusively for kernel errors and critical warnings:
$ sudo dmesg -T --level=err,crit

# 3. Check for Out-Of-Memory (OOM) killer terminations:
$ sudo dmesg -T | grep -i "oom"

# 4. Check for disk hardware and filesystem corruption errors:
$ sudo dmesg -T | grep -E "EXT4-fs error|I/O error|nvme"
```

---

## 3. Dynamic Kernel Tuning: `sysctl` & `/proc/sys`

The Linux kernel exposes hundreds of live tunable parameters inside the **`/proc/sys/`** directory. You can inspect and change them on-the-fly without rebooting.

### Inspecting Parameters:
```bash
# Query a specific kernel parameter:
$ sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 0

# Equivalent direct file read from procfs:
$ cat /proc/sys/net/ipv4/ip_forward
0
```

### Modifying Parameters Live in RAM:
```bash
# Enable IP packet forwarding (required for Kubernetes / Docker routing):
$ sudo sysctl -w net.ipv4.ip_forward=1
# OR:
$ echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

### Persisting Changes Across Reboots:
Changes made with `sysctl -w` vanish on reboot. To make them permanent:
1. Add configuration entries to `/etc/sysctl.d/99-custom.conf`:
   ```ini
   net.ipv4.ip_forward = 1
   vm.swappiness = 10
   fs.file-max = 2097152
   ```
2. Apply changes immediately without rebooting:
   ```bash
   $ sudo sysctl -p /etc/sysctl.d/99-custom.conf
   ```

---

## 4. Kernel Memory & Module Inspection Tools

```bash
# 1. Inspect kernel SLUB object caches in real time:
$ sudo slabtop -s c

# 2. List all loaded kernel modules sorted by size:
$ lsmod

# 3. Display detailed driver information and author of a module:
$ modinfo overlay

# 4. Load a kernel module with automatic dependency resolution:
$ sudo modprobe br_netfilter

# 5. Safely unload an idle kernel module:
$ sudo modprobe -r br_netfilter
```
