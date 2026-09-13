# 06 - Practical Commands for Inspecting Architecture & System Calls

A deep architectural understanding is worthless without knowing the tools to inspect system calls, kernel state, hardware devices, and memory buffers live.

---

## 1. Tracing System Calls with `strace`

`strace` intercepts and records the system calls called by a process and the signals received. It is the premier diagnostic tool for Linux engineers.

### Tracing File Copy Live:
```bash
# Trace only file-related system calls during a 'cp' execution:
$ strace -e trace=openat,read,write,close,copy_file_range cp /etc/hosts /tmp/hosts_copy
openat(AT_FDCWD, "/etc/hosts", O_RDONLY) = 3
openat(AT_FDCWD, "/tmp/hosts_copy", O_WRONLY|O_CREAT|O_TRUNC, 0666) = 4
copy_file_range(3, NULL, 4, NULL, 1073741824, 0) = 384
close(4)                                = 0
close(3)                                = 0
+++ exited with 0 +++
```

### Profiling Syscall Performance:
```bash
# Run a command and display a summary table of syscall time, calls, and errors:
$ strace -c ls -la /var/log
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 35.12    0.001420          14        98           newfstatat
 22.45    0.000908          18        50           getdents64
 18.20    0.000736          14        52           openat
  9.40    0.000380           7        52           close
 ...
```

### Attaching to a Live Stuck Process:
```bash
# Attach to an existing daemon (e.g. Nginx PID 4210) to see what system call it is blocked on:
$ sudo strace -p 4210
# (Press Ctrl+C to detach without killing the process)
```

---

## 2. Inspecting Hardware & CPU Architecture

```bash
# 1. Inspect CPU architecture, cores, sockets, caches, and flags:
$ lscpu
Architecture:            x86_64
CPU op-mode(s):          32-bit, 64-bit
Address sizes:           39 bits physical, 48 bits virtual
Byte Order:              Little Endian
CPU(s):                  8
Vendor ID:               GenuineIntel
Model name:              11th Gen Intel(R) Core(TM) i7-1165G7 @ 2.80GHz
Flags:                   fpu vme de pse tsc msr pae mce cx8 apic sep mtrr... vmx

# 2. Inspect PCI devices (Network cards, NVMe controllers, GPUs):
$ lspci -k | grep -A 2 -E "VGA|Ethernet|Non-Volatile"

# 3. Inspect USB devices:
$ lsusb
```

---

## 3. Kernel Subsystem & Module Inspection

```bash
# 1. Print full kernel release and compilation architecture:
$ uname -a
Linux ip-172-31-40-12 6.5.0-1014-aws #14-Ubuntu SMP x86_64 GNU/Linux

# 2. View kernel ring buffer boot logs and driver messages:
$ sudo dmesg -T --level=err,warn

# 3. List all active kernel modules and who uses them:
$ lsmod | head -n 10

# 4. View detailed information and parameters of a specific kernel driver:
$ modinfo e1000e
filename:       /lib/modules/.../kernel/drivers/net/ethernet/intel/e1000e/e1000e.ko
version:        3.2.6-k
license:        GPL
description:    Intel(R) PRO/1000 Network Driver
```

---

## 4. Measuring Context Switches & System States (`vmstat`)

`vmstat` (Virtual Memory Statistics) provides an instant snapshot of process states, memory pages, swap I/O, disk blocks, and CPU modes:

```bash
# Print system stats every 2 seconds:
$ vmstat 2
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 324100  45120 1854200   0    0     5    22  240  680  4  2 93  1  0
```

### Critical Columns for SREs:
- **`r` (Runnable):** Number of processes waiting for CPU time. If `r > CPU Cores`, CPU saturation is occurring.
- **`b` (Blocked):** Processes sleeping in uninterruptible sleep (usually waiting for disk I/O).
- **`cs` (Context Switches):** Number of times per second the CPU swapped tasks. Spikes (>50,000/sec) indicate heavy thread contention.
- **`in` (Interrupts):** Hardware and software interrupts per second.
- **`us` (User CPU %):** Time spent executing user space application code (Ring 3).
- **`sy` (System CPU %):** Time spent executing kernel code on behalf of system calls (Ring 0).
- **`wa` (I/O Wait %):** CPU idle time spent waiting for disk or network I/O.
- **`st` (Steal Time %):** Virtual CPU cycles stolen by the hypervisor for another tenant in Cloud VMs (AWS/GCP).
