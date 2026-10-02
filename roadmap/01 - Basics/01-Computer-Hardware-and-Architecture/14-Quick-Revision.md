# 14 — Quick Revision & Reference Cheat Sheet

A condensed, high-density reference sheet summarizing Computer Hardware and Architecture for rapid revision before interviews and production triage.

---

## 1. Latency Numbers Every DevOps / SRE Engineer Must Know

```text
[Registers]          0.5 - 1.0 ns  (1 cycle)
[L1 Cache]           1.0 - 2.0 ns  (3-5 cycles)
[L2 Cache]           3.0 - 7.0 ns  (10-15 cycles)
[L3 Cache]           10  - 25  ns  (40-60 cycles)
[Main Memory (DRAM)] 50  - 100 ns  (100x slower than L1)
[NVMe SSD (PCIe)]    10  - 30  µs  (100-300x slower than RAM)
[SATA SSD]           50  - 150 µs  (1,500x slower than RAM)
[Spinning HDD]       5   - 15  ms  (100,000x slower than RAM)
[US East to US West] 60  - 80  ms  (Internet Round-Trip)
```

> **Golden Rule of Performance:** When an application hits physical disk or makes a remote network hop, the CPU can execute tens of millions of instructions during that idle wait period.

---

## 2. Core Architecture Cheat Sheet

### Instruction Set Architectures (ISA)

| Feature | x86-64 / AMD64 | ARM64 / AArch64 | RISC-V |
| :--- | :--- | :--- | :--- |
| **Philosophy** | CISC | RISC | Modular RISC |
| **Instruction Length** | Variable (1 – 15 bytes) | Fixed (32 bits / 4 bytes) | Fixed (32 bits, optional compressed 16-bit) |
| **Memory Access** | Direct in arithmetic ops | Load / Store Only | Load / Store Only |
| **Licensing** | Proprietary (Intel/AMD) | Licensed IP (Arm Ltd) | Open Standard (Royalty-free) |
| **Cloud Examples** | AWS c6i, Azure D-series | AWS Graviton, GCP Tau T2A | Emerging IoT, AI Accelerators |

---

## 3. Virtual Memory & Paging Quick Map

```text
Virtual Address (App)
       │
       ▼
┌──────────────┐    TLB Hit? (1 cycle)
│     MMU      ├────────────────────────┐
└──────┬───────┘                        │
       │ TLB Miss                       ▼
       ▼                         [Physical Address]
┌──────────────┐                        │
│ Page Table   │                        ▼
│ Walk in RAM  │ ────► Page Present? ───┼─► [RAM Chip Frame]
└──────────────┘             │          ▲
                             │ No       │
                             ▼          │
                       [PAGE FAULT] ────┘
                       Kernel loads page from Disk/Swap
```

- **Standard Page Size:** 4 KB (4096 bytes).
- **Huge Pages:** 2 MB or 1 GB (reduces TLB footprint for large memory workloads).
- **OOM Killer:** Kernel safety net triggered when unevictable memory demands exceed physical RAM + Swap.

---

## 4. Storage Comparison Matrix

| Metric | SATA III HDD | SATA III SSD | NVMe SSD (PCIe Gen 4) |
| :--- | :--- | :--- | :--- |
| **Physical Bus** | SATA Cable (6 Gbps) | SATA Cable (6 Gbps) | Direct PCIe x4 Lanes (64 Gbps) |
| **Protocol** | AHCI | AHCI | NVMe |
| **Max Read Speed** | ~180 – 250 MB/s | ~550 MB/s | ~7,500 MB/s |
| **Max IOPS** | 75 – 200 IOPS | 50,000 – 90,000 IOPS | 800,000 – 1,500,000 IOPS |
| **Queue Architecture** | 1 queue, 32 commands | 1 queue, 32 commands | 64,000 queues, 64,000 commands/queue |
| **Latency** | 5 – 15 ms | 50 – 150 µs | 8 – 20 µs |

---

## 5. System Boot Sequence in 5 Steps

```text
1. Power-On & POST
   Motherboard initiates power rails; CPU starts at reset vector; runs Power-On Self-Test.

2. Firmware Execution (UEFI / BIOS)
   Initializes CPU registers, DRAM controller, and PCIe buses; locates boot device.

3. Bootloader (GRUB / systemd-boot)
   Firmware reads ESP partition, executes GRUB; GRUB presents menu, loads Kernel + Initramfs into RAM.

4. Kernel Initialization
   Kernel mounts initramfs as temporary root; detects drivers; mounts real root filesystem (`/`).

5. Init System (PID 1)
   Kernel launches systemd; systemd brings up services, targets, networking, and login prompts.
```

---

## 6. Hardware Virtualization vs Containers

```text
   VIRTUAL MACHINES (Type 1)                    CONTAINERS (Linux)
+-------------------------------+       +-------------------------------+
| App A | App B | App C         |       | App A | App B | App C         |
+-------+-------+---------------+       +-------+-------+---------------+
| Guest OS Kernel & Drivers     |       | App Binaries & Libraries      |
+-------------------------------+       +-------------------------------+
| Hypervisor (KVM / ESXi)       |       | Container Runtime (containerd)|
+-------------------------------+       +-------------------------------+
| Physical Server Hardware      |       | Host Linux Kernel (Namespaces)|
+-------------------------------+       +-------------------------------+
                                        | Physical Server Hardware      |
                                        +-------------------------------+
```

- **VM:** Hardware-level isolation. Virtualizes CPU, Memory, and Devices. Heavy (GBs RAM, minutes to boot).
- **Container:** OS-level isolation. Shares host kernel. Process-level boundaries (MBs RAM, milliseconds to boot).

---

## 7. Essential Hardware Diagnostic Commands

| Task | Command |
| :--- | :--- |
| **CPU Architecture & Caches** | `lscpu` |
| **Live Core Frequency & Flags** | `cat /proc/cpuinfo` |
| **PCIe Bus Devices (NIC, GPU)** | `lspci -tv` |
| **Block Storage Topology & Rotational**| `lsblk -o NAME,SIZE,TYPE,ROTA,MOUNTPOINT` |
| **RAM Usage & Buffers** | `free -h` |
| **Real-time Memory Breakdown** | `cat /proc/meminfo` |
| **Hardware IRQ Distribution** | `cat /proc/interrupts` |
| **Motherboard & BIOS Firmware** | `sudo dmidecode -t bios` |
| **Disk IOPS & Latency Bottlenecks**| `iostat -xz 1` |
| **CPU Steal & Hypervisor Contention**| `mpstat -P ALL 1` |
| **NUMA Topology & Memory Locality** | `numactl --hardware` |
| **Hardware Virtualization Check** | `egrep -c '(vmx\|svm)' /proc/cpuinfo` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 13 - MCQ](./13-MCQ.md) | [Index](../../../README.md) | [Next Module (02-OS-and-Kernel-Fundamentals) →](../02-OS-and-Kernel-Fundamentals/01-OS-Architecture-Kernel-Types-and-Dual-Mode.md) |
