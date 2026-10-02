# 01 - Computer Hardware and Architecture

> Complete Deep-Dive Guide to Computer Architecture, CPU Mechanics, Memory Hierarchy, Storage Subsystems, Motherboard Buses, Firmware, and Hardware Virtualization for DevOps and Cloud Engineers.

---

## 🗺️ Architectural Mind Map

```text
                                  COMPUTER SYSTEM
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
        │                                │                                │
 ┌──────┴──────┐                  ┌──────┴──────┐                  ┌──────┴──────┐
 │     CPU     │                  │   Memory    │                  │ Storage &   │
 │             │                  │  Hierarchy  │                  │     I/O     │
 └──────┬──────┘                  └──────┬──────┘                  └──────┬──────┘
        │                                │                                │
 ┌──────┼──────┐                  ┌──────┼──────┐                  ┌──────┼──────┐
 │      │      │                  │      │      │                  │      │      │
CU     ALU  Registers          L1/L2/L3 RAM    TLB                NVMe   PCIe   NIC
 │      │      │                  │      │      │                  │      │      │
PC     IR     SP                Caches DRAM Virtual               SSD   DMA    MAC
                                              Memory
```

---

## 📌 Complete Syllabus Breakdown (All 74 Topics)

| Module File | Topics Covered | Key Focus Areas |
|---|---|---|
| [`01-Computer-Architecture-and-CPU-Fundamentals.md`](01-Computer-Architecture-and-CPU-Fundamentals.md) | **Topics 1–13** | Computer Fundamentals, Von Neumann Architecture, CPU, Control Unit, ALU, Registers (PC, IR, SP), Cores, Threads, Clock Speed, Instruction Cycle. |
| [`02-ISA-x86-ARM-RISCV-and-Microarchitecture.md`](02-ISA-x86-ARM-RISCV-and-Microarchitecture.md) | **Topics 14–21, 74** | Instruction Set Architecture (ISA), x86 vs x86-64/AMD64, ARM vs ARM64/AArch64, RISC-V, Microarchitecture, IPC, Software Compatibility. |
| [`03-CPU-Cache-Hierarchy-and-Memory-Subsystems.md`](03-CPU-Cache-Hierarchy-and-Memory-Subsystems.md) | **Topics 22–29** | CPU Cache, L1/L2/L3 Cache, Cache Hit/Miss, RAM, DRAM vs SRAM, Memory Latency. |
| [`04-Virtual-Memory-Paging-and-MMU.md`](04-Virtual-Memory-Paging-and-MMU.md) | **Topics 30–38** | Virtual Memory, Physical Memory, Virtual vs Physical Address, Pages, Page Tables, Page Faults, TLB, Memory Protection. |
| [`05-Storage-Technologies-and-IO-Subsystems.md`](05-Storage-Technologies-and-IO-Subsystems.md) | **Topics 39–44** | HDD, SSD, SATA SSD, NVMe SSD, Storage I/O, Input/Output Subsystem. |
| [`06-Motherboard-Buses-and-Peripherals.md`](06-Motherboard-Buses-and-Peripherals.md) | **Topics 45–56** | Motherboard, PCIe, USB, SATA Interface, GPU, NIC, MAC Address, Interrupts & Handling, DMA, System Bus, Memory Bus. |
| [`07-Firmware-BIOS-UEFI-and-Boot-Process.md`](07-Firmware-BIOS-UEFI-and-Boot-Process.md) | **Topics 57–63** | Firmware, BIOS vs UEFI, Boot Process, Bootloader, GRUB, Windows Boot Manager. |
| [`08-Virtualization-Hypervisors-and-Containers.md`](08-Virtualization-Hypervisors-and-Containers.md) | **Topics 64–73** | Hardware Virtualization, Intel VT-x, AMD-V, Hypervisor (Type 1 & Type 2), VMs, Virtualization vs Containers. |
| [`09-Real-World-Scenarios.md`](09-Real-World-Scenarios.md) | Production Patterns | AWS Graviton Migration, NVMe IOPS Bottlenecks, GPU Cloud Sizing for LLM Inference. |
| [`10-Troubleshooting.md`](10-Troubleshooting.md) | Diagnostic Runbook | CPU Throttling, Memory Leaks, High `iowait`, PCIe Bus Errors, Hardware MCEs. |
| [`11-Interview-QA.md`](11-Interview-QA.md) | Technical Interview Prep | 20+ Real-world interview questions and detailed answers. |
| [`12-Hands-On-Practice.md`](12-Hands-On-Practice.md) | Practical Labs | 10 CLI Labs inspecting CPU, memory, disks, PCIe, and virtualization flags. |
| [`13-MCQ.md`](13-MCQ.md) | Self-Assessment | 15+ Diagnostic MCQs with complete answer keys and rationales. |
| [`14-Quick-Revision.md`](14-Quick-Revision.md) | Fast Recall Sheet | One-page cheat sheet, latency numbers, and architecture comparison tables. |
