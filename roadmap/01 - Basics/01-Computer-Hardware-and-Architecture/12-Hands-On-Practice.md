# 12 — Hands-On Practice Labs: Hardware & Architecture

Practical, command-driven laboratory exercises to inspect, profile, benchmark, and troubleshoot hardware subsystems on Linux servers.

---

## Lab 1: Comprehensive Hardware Discovery & Inventory

### Objective
Audit all hardware components of a host or virtual machine without opening the chassis or accessing the cloud web console.

### Commands to Execute
```bash
# 1. Inspect CPU microarchitecture, cores, threads, and cache topology
lscpu

# 2. View summary of all physical hardware (requires root)
sudo lshw -short

# 3. List all devices connected to the PCIe bus (NICs, GPUs, NVMe drives)
lspci -tv

# 4. View detailed block storage topology with partition sizes and types
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT,ROTA,MODEL

# 5. Extract motherboard and BIOS firmware version from SMBIOS tables
sudo dmidecode -t bios
sudo dmidecode -t baseboard
```

### Expected Output & Key Checks
- Look at `ROTA` column in `lsblk`: `0` indicates solid-state drive (SSD/NVMe), `1` indicates rotating disk (HDD).
- In `lscpu`, verify:
  - `Model name` (Intel Xeon, AMD EPYC, or ARM Neoverse)
  - `Thread(s) per core`: `2` indicates Simultaneous Multithreading (Hyper-Threading); `1` indicates physical cores only.
  - `L1d`, `L1i`, `L2`, and `L3 cache` sizes.

---

## Lab 2: Deep Inspection of `/proc` Hardware Interfaces

### Objective
Extract raw runtime hardware telemetry directly from the Linux kernel pseudo-filesystem.

### Commands to Execute
```bash
# 1. Inspect per-core flags for hardware virtualization and security mitigations
cat /proc/cpuinfo | grep -E "model name|flags" | head -n 4

# Check specifically for hardware virtualization support:
egrep -q '(vmx|svm)' /proc/cpuinfo && echo "Hardware Virtualization Supported" || echo "Not Supported"

# 2. Inspect real-time memory allocations (Available vs Free)
cat /proc/meminfo | grep -E "MemTotal|MemAvailable|MemFree|Buffers|Cached|SwapTotal|SwapFree|HugePages_Total"

# 3. View interrupt counts per core to audit IRQ distribution
cat /proc/interrupts | head -n 15

# 4. Check PCIe direct memory access and I/O memory map
sudo head -n 20 /proc/iomem
```

### Analysis Checklist
- **MemFree vs MemAvailable:** Never rely solely on `MemFree`. Modern Linux caches disk pages aggressively in memory. `MemAvailable` is the true metric indicating how much memory can be granted to applications without swapping.
- **Interrupts:** Check if hardware interrupts (e.g., `eth0` or `nvme0q1`) are balanced across all CPU columns or concentrated on `CPU0`.

---

## Lab 3: Memory Bandwidth & CPU Cache Thrashing with `perf`

### Objective
Observe how CPU cache hits and misses impact application execution cycles.

### Prerequisites
Install linux tools and perf:
```bash
sudo apt update && sudo apt install -y linux-tools-common linux-tools-generic sysbench
# Or on RHEL/Rocky:
# sudo dnf install -y perf sysbench
```

### Commands to Execute
```bash
# 1. Run a 10-second multi-threaded CPU benchmark
sysbench cpu --cpu-max-prime=20000 --threads=$(nproc) run

# 2. Record CPU hardware cache miss events during benchmark
perf stat -e cycles,instructions,cache-references,cache-misses,dTLB-load-misses \
  sysbench cpu --cpu-max-prime=20000 --threads=2 run
```

### Metrics Interpretation
- **IPC (Instructions Per Cycle):** Calculated as `instructions / cycles`. An IPC > 1.5 indicates highly efficient, non-stalled pipelining. An IPC < 0.7 indicates the CPU is stalling, waiting for memory from RAM or cache misses.
- **Cache-misses percentage:** Ideally below 5–10% for compute-bound algorithms.

---

## Lab 4: Storage IOPS & Latency Benchmarking with `fio`

### Objective
Measure true hardware IOPS, queue depth handling, and latency of a storage volume (NVMe, SSD, or Cloud EBS) to establish performance baselines.

### Prerequisites
```bash
sudo apt install -y fio
```

### Test 1: Random 4K Read IOPS (Simulating High-Concurrency OLTP Database)
```bash
fio --name=random-read-iops \
  --filename=/tmp/fio-test-file \
  --rw=randread \
  --bs=4k \
  --ioengine=libaio \
  --iodepth=64 \
  --numjobs=4 \
  --direct=1 \
  --runtime=20 \
  --time_based \
  --group_reporting \
  --size=1G
```

### Test 2: Sequential 1MB Write (Simulating High-Throughput Backup or Logging)
```bash
fio --name=seq-write-bandwidth \
  --filename=/tmp/fio-test-file \
  --rw=write \
  --bs=1M \
  --ioengine=libaio \
  --iodepth=16 \
  --numjobs=1 \
  --direct=1 \
  --runtime=20 \
  --time_based \
  --group_reporting \
  --size=1G

# Clean up test file
rm -f /tmp/fio-test-file
```

### Key Metrics to Record
- **IOPS:** Total Input/Output Operations Per Second.
- **Latency (lat):** Record `p95` and `p99` latency in milliseconds or microseconds.
- **Direct I/O (`--direct=1`):** Ensures tests bypass the OS page cache, benchmarking actual physical storage hardware.

---

## Lab 5: NUMA Node Profiling and Process Affinity Pinning

### Objective
Examine NUMA topology on a multi-core machine and enforce strict CPU core and memory node binding.

### Commands to Execute
```bash
# 1. Install numactl
sudo apt install -y numactl

# 2. Audit NUMA hardware nodes and core mappings
numactl --hardware

# 3. Check memory distribution per NUMA node
numastat

# 4. Launch a process pinned strictly to NUMA Node 0 (both CPU and Memory)
numactl --cpunodebind=0 --membind=0 sleep 60 &
PID=$!

# 5. Verify process affinity mask using taskset
taskset -p $PID

# Kill process
kill -9 $PID
```

---

## Lab 6: Hardware Virtualization Readiness Audit

### Objective
Verify if a host is ready to run hardware-accelerated hypervisors (KVM, QEMU, Minikube, Docker Desktop).

### Commands to Execute
```bash
# 1. Check for KVM kernel device existence
ls -l /dev/kvm

# 2. Run official virtualization validation script
sudo apt install -y libvirt-clients
virt-host-validate
```

### Pass Criteria
- `/dev/kvm` must exist with permissions `crw-rw---- 1 root kvm`.
- `virt-host-validate` should report `PASS` for `QEMU: Checking for hardware virtualization`.
