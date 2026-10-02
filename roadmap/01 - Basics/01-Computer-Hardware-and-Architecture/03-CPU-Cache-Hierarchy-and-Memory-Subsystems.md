# 03 - CPU Cache Hierarchy & Memory Subsystems

---

## 1. The Memory Latency Gap (The Hardware Reality)

While CPU execution core frequencies have accelerated thousands of times over the past four decades, physical memory (DRAM) access latencies have improved at a drastically slower pace. If a modern CPU had to wait for physical RAM on every single instruction, it would sit idle for over **95% of its operating cycles**.

To bridge this chasm, modern hardware relies on a **hierarchical caching system** based upon two fundamental physical laws:
1. **Temporal Locality:** If a memory location is accessed once, it is overwhelmingly likely to be accessed again in the near future (e.g., loop variables, active stack frames).
2. **Spatial Locality:** If a memory location is accessed, memory locations immediately adjacent to it are likely to be accessed soon (e.g., sequential array iterations, contiguous program instructions).

```text
Latency Numbers Every DevOps & SRE Engineer Must Know:
┌─────────────────────────────────┬──────────────────────────┬──────────────────────────┐
│ Storage Hierarchy Level         │ Access Latency           │ Scaled to Human Scale    │
├─────────────────────────────────┼──────────────────────────┼──────────────────────────┤
│ CPU Registers                   │ 0.3 - 0.5 nanoseconds    │ 1 second                 │
│ L1 Cache (Instruction & Data)   │ 1 - 1.5 nanoseconds      │ 3 seconds                │
│ L2 Cache                        │ 3 - 5 nanoseconds        │ 10 seconds               │
│ L3 Cache (Shared LLC)           │ 10 - 20 nanoseconds      │ 40 seconds               │
│ Main Memory (DRAM)              │ 60 - 100 nanoseconds     │ 3.5 minutes              │
│ NVMe SSD (I/O Bus)              │ 10 - 50 microseconds     │ 1.5 days                 │
│ SATA SSD                        │ 100 - 200 microseconds   │ 6 days                   │
│ Mechanical Hard Disk (HDD)      │ 5 - 10 milliseconds      │ 6 months                 │
│ Network Packet (Cross-Datacenter)│ 30 - 100 milliseconds   │ 2 - 6 years              │
└─────────────────────────────────┴──────────────────────────┴──────────────────────────┘
```

---

## 2. On-Die CPU Cache Hierarchy (L1, L2, L3)

```text
┌───────────────────────────────────────────────────────────────┐
│ CPU Package                                                   │
│   ┌───────────────────────────┐   ┌─────────────────────────┐ │
│   │ Core 0                    │   │ Core 1                  │ │
│   │   ┌───────────────────┐   │   │   ┌─────────────────┐   │ │
│   │   │ L1i (32-64 KB)    │   │   │   │ L1i (32-64 KB)  │   │ │
│   │   ├───────────────────┤   │   │   ├─────────────────┤   │ │
│   │   │ L1d (32-64 KB)    │   │   │   │ L1d (32-64 KB)  │   │ │
│   │   └─────────┬─────────┘   │   │   └────────┬────────┘   │ │
│   │             ▼             │   │            ▼            │ │
│   │   ┌───────────────────┐   │   │   ┌─────────────────┐   │ │
│   │   │ L2 Cache (1-2 MB) │   │   │   │ L2 Cache (1-2MB)│   │ │
│   │   └─────────┬─────────┘   │   │   └────────┬────────┘   │ │
│   └─────────────┼─────────────┘   └────────────┼────────────┘ │
│                 │                              │              │
│                 ▼                              ▼              │
│       ┌───────────────────────────────────────────────┐       │
│       │           Shared L3 Cache (16 - 128+ MB)      │       │
│       └───────────────────────┬───────────────────────┘       │
└───────────────────────────────┼───────────────────────────────┘
                                ▼
                   Main System Memory (DRAM)
```

### Level 1 Cache (L1)
- Resides directly inside each physical core.
- Divided into two specialized sub-caches to prevent pipeline structural hazards:
  - **L1i (L1 Instruction Cache):** Holds pre-decoded machine instructions.
  - **L1d (L1 Data Cache):** Holds active working data variables.
- Size: Typically 32 KB to 64 KB per core. Latency: ~1 to 1.5 ns (4 to 5 CPU cycles).

### Level 2 Cache (L2)
- Dedicated private cache for each physical core (in modern architectures).
- Larger and slightly slower than L1; serves as the immediate buffer when an L1 miss occurs.
- Size: Typically 512 KB to 2 MB per core. Latency: ~3 to 5 ns (12 to 14 CPU cycles).

### Level 3 Cache (L3 / Last Level Cache - LLC)
- Shared across all cores within a CPU socket or Core Complex (CCX).
- Ensures multi-threaded processes running on different cores can share cache lines without round-tripping to main memory.
- Size: Typically 16 MB to 128 MB (and up to 768 MB in AMD 3D V-Cache server CPUs). Latency: ~10 to 20 ns (40 to 60 CPU cycles).

---

## 3. Cache Lines, Cache Hits & Cache Misses

- **Cache Line:** The atomic unit of data transfer between memory and cache. Almost all modern CPUs use a **64-byte cache line**. Even if an instruction requests a single 1-byte character, the hardware loads 64 contiguous bytes into cache!
- **Cache Hit:** The requested memory address is already resident in L1/L2/L3 cache. The CPU continues execution with zero pipeline stalls.
- **Cache Miss:** The requested memory address is NOT in cache. The core must stall execution pipelines while the memory controller fetches the cache line from DRAM, incurring massive latency penalties.
- **Cache Coherency (MESI Protocol):** In multi-core systems, if Core 0 modifies variable `X` in its private L1 cache, hardware cache coherency protocols (Modified, Exclusive, Shared, Invalid) broadcast invalidate signals across the bus to ensure Core 1 does not read stale data.

---

## 4. Main Memory: RAM, DRAM vs. SRAM

### SRAM (Static Random Access Memory)
- **Cell Design:** Constructed using 6 transistors (6T) per bit.
- **Behavior:** Retains data continuously as long as power is applied without needing periodic refreshing.
- **Characteristics:** Ultra-fast, highly expensive, physically large footprint. Used exclusively for on-die CPU caches (L1/L2/L3).

### DRAM (Dynamic Random Access Memory)
- **Cell Design:** Constructed using 1 transistor and 1 capacitor (1T-1C) per bit.
- **Behavior:** The capacitor stores an electrical charge representing 1 or 0, but this charge rapidly leaks away within milliseconds. DRAM requires a memory controller to continuously **refresh** (recharge) every cell thousands of times per second.
- **Characteristics:** Denser, significantly cheaper, lower power. Forms the bulk physical system RAM (DDR4, DDR5 DIMMs).

---

## 5. Non-Uniform Memory Access (NUMA) in Multi-Socket Cloud Servers

In modern multi-socket enterprise servers (e.g., dual Intel Xeon or AMD EPYC servers), physical RAM is partitioned across sockets into **NUMA Nodes**:

```text
┌───────────────────────────────┐               ┌───────────────────────────────┐
│ NUMA Node 0                   │               │ NUMA Node 1                   │
│ ┌───────────┐   ┌───────────┐ │               │ ┌───────────┐   ┌───────────┐ │
│ │ Socket 0  │<->│ Local RAM │ │<═════════════>│ │ Socket 1  │<->│ Local RAM │ │
│ └───────────┘   └───────────┘ │ Inter-Socket  │ └───────────┘   └───────────┘ │
└───────────────────────────────┘ Bus (UPI/IF)  └───────────────────────────────┘
```
- **Local Access:** A CPU core accessing RAM attached directly to its own socket experiences lowest latency (~60 ns).
- **Remote Access:** A CPU core accessing RAM attached to the opposite socket must traverse the inter-socket interconnect (Intel UPI / AMD Infinity Fabric), suffering **2x to 3x higher latency**.
- **DevOps Impact:** High-performance database pods (PostgreSQL, Redis) and Kubernetes worker nodes should use **NUMA pinning (Topology Manager)** to lock containers to specific NUMA nodes, avoiding remote memory penalties.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - ISA x86 ARM RISCV and Microarchitecture](./02-ISA-x86-ARM-RISCV-and-Microarchitecture.md) | [README](./README.md) | [04 - Virtual Memory Paging and MMU](./04-Virtual-Memory-Paging-and-MMU.md) |
