# Computer Hardware and Architecture — Core Concepts

## 1. What Is a Computer?

A computer is an electronic system that accepts input data, processes it according to stored instructions, holds intermediate data in volatile memory or persistent storage, and produces output results.

### Basic Data Flow

```text
Input Device (Keyboard / Network / API)
         ↓
    Processing (CPU)
         ↓
  Memory / Storage (RAM / SSD)
         ↓
Output Device (Display / Network Response)
```

---

## 2. Computer Architecture & Von Neumann Model

Computer architecture defines how CPU, memory, and I/O devices are organized and connected via system buses.

```text
                        ┌───────────────────────────────┐
                        │           COMPUTER            │
                        └───────────────┬───────────────┘
                                        │
             ┌──────────────────────────┼──────────────────────────┐
             │                          │                          │
      ┌──────┴──────┐            ┌──────┴──────┐            ┌──────┴──────┐
      │     CPU     │            │   Memory    │            │     I/O     │
      └──────┬──────┘            └──────┬──────┘            └──────┬──────┘
  ┌──────────┼──────────┐               │               ┌──────────┼──────────┐
  │          │          │              RAM              │          │          │
 ALU        CU      Registers                         Keyboard   Mouse     Display
  │                                                     │          │          │
  └─────────────────────────── Storage ─────────────────┴──────────┴──────────┘
                              (SSD / HDD)
```

---

## 3. CPU (Central Processing Unit)

The CPU is the primary processing engine of a computer. It fetches instructions from RAM, decodes them into micro-operations, and executes them.

### Major Internal Components

1. **Control Unit (CU):** Coordinates CPU operations, fetches instructions, decodes opcodes, and directs signal execution.
2. **Arithmetic Logic Unit (ALU):** Performs mathematical calculations (`+`, `-`, `*`, `/`) and logical operations (`AND`, `OR`, `XOR`, `NOT`, comparisons).
3. **Registers:** Extremely small, high-speed storage slots inside the processor core.
   - **Program Counter (PC):** Holds the memory address of the next instruction to execute.
   - **Instruction Register (IR):** Holds the current instruction being decoded/executed.
   - **Stack Pointer (SP):** Points to the current top address of the execution stack.
   - **Status/Flags Register:** Stores condition flags (e.g., zero flag, carry flag, overflow flag).
4. **Cache (L1, L2, L3):** Ultra-fast SRAM memory layers placed on-die to reduce memory latency.
5. **Cores & Execution Units:** Independent processing cores capable of concurrent execution.
6. **Clock:** Generates clock pulses (GHz) to synchronize state transitions across internal circuits.

### CPU Execution Cycle (Fetch-Decode-Execute)

```text
┌─────────┐      ┌──────────┐      ┌─────────┐      ┌─────────┐
│  Fetch  │ ───> │  Decode  │ ───> │ Execute │ ───> │  Store  │ ───┐
└─────────┘      └──────────┘      └─────────┘      └─────────┘    │
     ▲                                                             │
     └─────────────────────────────────────────────────────────────┘
```

---

## 4. CPU Cores, Threads & Clock Speed

- **CPU Core:** An independent hardware execution core containing its own registers and L1/L2 caches.
- **Hardware Thread / Hyper-Threading (SMT):** Allows a single physical core to present two virtual cores to the operating system, sharing pipeline execution units.
- **Process vs Thread:**
  - **Process:** Isolated container owned by OS with dedicated virtual address space and file descriptors.
  - **Thread:** Lightweight execution context within a process, sharing memory address space.
- **Clock Speed (GHz):** Frequency of CPU clock cycles (e.g., 3.5 GHz = 3.5 billion cycles per second). Performance depends on Clock Speed $\times$ Instructions Per Cycle (IPC) $\times$ Core count.

---

## 5. Instruction Set Architectures (ISA): x86-64 vs ARM64

| Feature | x86-64 (amd64) | ARM64 (aarch64) | RISC-V |
|---|---|---|---|
| **Design Type** | CISC (Complex Instruction Set) | RISC (Reduced Instruction Set) | Open RISC Standard |
| **Power Efficiency** | Moderate to High Power | Exceptionally High Efficiency | Highly Configurable |
| **Cloud Providers** | AWS EC2 (Intel/AMD), Azure, GCP | AWS Graviton, Ampere Altra (OCI, GCP, Azure) | Emerging Edge/Cloud |
| **DevOps Impact** | Default compiled binary target | Requires multi-arch Docker images (`docker buildx`) | Custom embedded/IoT targets |

> **Critical DevOps Rule:** A binary compiled natively for `x86_64` cannot execute on an `arm64` CPU without emulation (e.g., QEMU or Rosetta). Always build multi-architecture container images!

---

## 6. Memory Hierarchy & Virtual Memory

```text
    ┌───────────────────────────┐  Fastest / Smallest
    │       CPU Registers       │  (Sub-nanosecond)
    ├───────────────────────────┤
    │      L1 / L2 / L3 Cache   │  (1 - 10 ns)
    ├───────────────────────────┤
    │     Physical RAM (DRAM)   │  (50 - 100 ns)
    ├───────────────────────────┤
    │      NVMe / SATA SSD      │  (10 - 100 µs)
    ├───────────────────────────┤
    │      HDD / Cloud Storage  │  Slowest / Largest (ms)
    └───────────────────────────┘
```

- **RAM (Random Access Memory):** Volatile, high-speed primary memory holding active processes.
- **Virtual Memory:** OS abstraction mapping process virtual addresses to physical RAM pages using the **Memory Management Unit (MMU)**.
- **Paging:** Memory divided into fixed 4 KB blocks called pages.
- **TLB (Translation Lookaside Buffer):** CPU hardware cache holding recent Virtual-to-Physical page table mappings.

---

## 7. Persistent Storage: HDD, SSD & NVMe

- **HDD (Hard Disk Drive):** Magnetic spinning platters; high latency (~5-15 ms), low random IOPS.
- **SATA SSD:** NAND flash over legacy SATA controller; max throughput ~550 MB/s, ~5,000 IOPS.
- **NVMe SSD:** NAND flash over PCIe bus directly; throughput 3,500 - 14,000+ MB/s, 500,000+ IOPS, microsecond latency.

---

## 8. Virtualization: Hypervisors, VMs & Containers

```text
┌────────────────────────────────┐    ┌────────────────────────────────┐
│         App A   │  App B       │    │   Container A  │ Container B   │
├────────────────────────────────┤    ├────────────────────────────────┤
│       Guest OS  │  Guest OS    │    │      Container Runtime         │
├────────────────────────────────┤    ├────────────────────────────────┤
│           Hypervisor           │    │           Host OS              │
├────────────────────────────────┤    ├────────────────────────────────┤
│        Physical Hardware       │    │       Physical Hardware        │
└────────────────────────────────┘    └────────────────────────────────┘
       Virtual Machine (VM)                       Container
```

- **Type 1 Hypervisor (Bare-Metal):** Runs directly on hardware (VMware ESXi, KVM, Hyper-V, Xen).
- **Type 2 Hypervisor (Hosted):** Runs on top of OS (VirtualBox, VMware Workstation).
- **Hardware Assist:** Intel VT-x / AMD-V CPU instructions accelerate VM context switches.
- **VM vs Container:** VMs virtualize hardware and include full Guest OS. Containers share the Host OS Kernel using Linux Namespaces and Cgroups.
