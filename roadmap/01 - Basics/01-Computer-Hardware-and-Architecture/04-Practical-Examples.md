# Hardware & Architecture — Practical CLI Commands & Inspection

## 1. Inspecting CPU Architecture & Cores

```bash
# Detailed CPU architecture report
lscpu

# View raw CPU information from kernel proc filesystem
cat /proc/cpuinfo | grep -E "model name|cpu cores|siblings|flags"

# Print architecture name (x86_64 or aarch64)
uname -m
```

### Sample Output Interpretation (`lscpu`)

```text
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         39 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  8
  On-line CPU(s) list:   0-7
Vendor ID:               GenuineIntel
  Model name:            11th Gen Intel(R) Core(TM) i7-1165G7 @ 2.80GHz
    Thread(s) per core:  2
    Core(s) per socket:  4
    Socket(s):           1
Virtualization features: 
  Virtualization:        VT-x
Caches (sum of all):     
  L1d:                   192 KiB (4 instances)
  L1i:                   128 KiB (4 instances)
  L2:                    5 MiB (4 instances)
  L3:                    12 MiB (1 instance)
```

---

## 2. Inspecting Memory (RAM, Swap, Cache)

```bash
# Display total, used, free, and cached memory in human-readable units
free -h -t

# Detailed kernel memory breakdown
cat /proc/meminfo | head -n 15

# Monitor memory and swap usage continuously
vmstat 1 5
```

---

## 3. Inspecting Storage & NVMe Devices

```bash
# List block devices with filesystem types and mount points
lsblk -f

# Check disk space utilization
df -hT

# Inspect NVMe device details (requires nvme-cli)
sudo nvme list
sudo nvme smart-log /dev/nvme0n1
```

---

## 4. Inspecting PCI Devices & Network Cards

```bash
# List all PCI buses and connected devices (NICs, GPUs, NVMe controllers)
lspci -vmm

# Inspect network interface hardware details
ip -c link show
ethtool eth0
```

---

## 5. Identifying Virtualization & Cloud Hypervisor Environment

```bash
# Detect whether running inside a VM or container
systemd-detect-virt

# Check DMI/SMBIOS hardware information (requires root)
sudo dmidecode -s system-product-name
sudo dmidecode -s system-manufacturer
```
