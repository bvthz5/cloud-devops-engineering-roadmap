# 15 - Hands-On Practice & Kernel Terminal Labs

Execute these practical terminal exercises to inspect kernel subsystems, tune parameters, and manage kernel modules live.

---

## 🧪 Lab 1: Live Kernel Tuning via `sysctl` and `procfs`

### Objective:
Inspect and tune kernel memory variables live in RAM and make them persistent.

```bash
# 1. Check current swappiness value (tendency to swap anonymous memory):
cat /proc/sys/vm/swappiness
# (Default is typically 60)

# 2. Modify the kernel parameter live using sysctl:
sudo sysctl -w vm.swappiness=10

# 3. Verify the procfs virtual file updated immediately:
cat /proc/sys/vm/swappiness
# Output: 10

# 4. Make the parameter persistent across reboots:
echo "vm.swappiness = 10" | sudo tee /etc/sysctl.d/99-swappiness.conf

# 5. Reload sysctl configuration:
sudo sysctl -p /etc/sysctl.d/99-swappiness.conf
```

---

## 🧪 Lab 2: Kernel SLUB Memory Inspection with `slabtop`

### Objective:
Observe the kernel's internal object pools and identify memory consumption by inodes and dentries.

```bash
# 1. Launch slabtop sorted by cache size:
sudo slabtop -s c

# Look closely at:
# - 'inode_cache': Kernel memory used to hold inode metadata
# - 'dentry': Kernel memory used to hold directory path mappings
# - 'buffer_head': Metadata headers for active I/O buffers

# 2. Generate directory activity to see dentries grow:
ls -laR /usr > /dev/null

# 3. Re-run slabtop to see dentry count increase:
sudo slabtop -s c | head -n 10
```

---

## 🧪 Lab 3: Tracking Kernel Modules & Dependency Trees

### Objective:
Inspect the kernel's module dependency tree and see how `modprobe` resolves prerequisites.

```bash
# 1. Print the running kernel release:
KERNEL_VER=$(uname -r)
echo "Current kernel: $KERNEL_VER"

# 2. Inspect the kernel's binary dependency database:
cat /lib/modules/$KERNEL_VER/modules.dep | grep "overlay.ko"

# 3. Load a test module with verbose dependency tracing (-v):
sudo modprobe -v dummy

# 4. Confirm the module is actively managed in kernel memory:
lsmod | grep dummy

# 5. Remove the module:
sudo modprobe -rv dummy
```

---

## 🧪 Lab 4: Filtering Kernel Ring Buffer Logs with `dmesg`

### Objective:
Filter low-level kernel event messages by severity and facility.

```bash
# 1. View all kernel warnings and errors since boot:
sudo dmesg -T --level=err,warn

# 2. Inspect CPU core initialization messages:
sudo dmesg -T | grep -i "smpboot: CPU"

# 3. Inspect storage controller detection:
sudo dmesg -T | grep -E "ahci|nvme|scsi"
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Interview Q&A](./14-Interview-Q&A.md) | [README](./README.md) | [16 - MCQ](./16-MCQ.md) |
