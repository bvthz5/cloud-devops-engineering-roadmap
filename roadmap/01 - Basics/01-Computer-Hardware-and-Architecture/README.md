# 01-Computer Hardware and Architecture

> Essential hardware components, CPU mechanics, memory hierarchy, storage technologies, architectures (x86 vs ARM), and virtualization foundations for Cloud & DevOps Engineers.

---

## 🎯 Learning Objectives

By the end of this module, you will understand:
1. **Computer System Flow:** How data moves across Input, CPU Processing, Memory, Storage, and Output.
2. **CPU Internals:** Control Unit (CU), Arithmetic Logic Unit (ALU), Registers (PC, SP, IR), L1/L2/L3 Caches, Cores, Threads, and Clock Speed.
3. **CPU Architectures & ISAs:** x86-64 (amd64) vs ARM64 (aarch64) vs RISC-V and why architecture matters in Docker, Kubernetes, and Cloud Machine Selection.
4. **Memory Hierarchy & Virtual Memory:** RAM volatility, Paging, Page Tables, TLB (Translation Lookaside Buffer), and Swap.
5. **Persistent Storage Technologies:** HDD vs SATA SSD vs NVMe SSD over PCIe, I/O performance, Interrupts, DMA, and Buses.
6. **Boot Process & Firmware:** Motherboard interfaces, BIOS vs UEFI, Bootloaders (GRUB, systemd-boot), GPUs/CUDA, and NICs/MAC addresses.
7. **Virtualization Foundations:** Hardware virtualization (VT-x, AMD-V), Type 1 vs Type 2 Hypervisors, VMs vs Containers.

---

## 📁 Module Navigation

- [`01-Basics.md`](01-Basics.md) — Core concepts, CPU breakdown, RAM, storage, and I/O.
- [`02-Deep-Dive.md`](02-Deep-Dive.md) — Deep architectural mechanics, TLB, Cache L1/L2/L3, NVMe/PCIe, ISAs.
- [`03-How-It-Works.md`](03-How-It-Works.md) — Step-by-step execution cycles, boot flow diagrams, VM vs Container execution.
- [`04-Practical-Examples.md`](04-Practical-Examples.md) — Hardware inspection CLI runbooks (`lscpu`, `free`, `lsblk`, `lspci`, `uname`).
- [`05-Real-World-Scenarios.md`](05-Real-World-Scenarios.md) — Selecting cloud instance types (Graviton ARM vs x86), NVMe I/O tuning, GPU allocation.
- [`06-Troubleshooting.md`](06-Troubleshooting.md) — Diagnosing hardware bottlenecks, memory leaks, I/O wait (`iowait`), and thermal throttling.
- [`07-Interview-QA.md`](07-Interview-QA.md) — Technical interview Q&A for hardware & virtualization.
- [`08-MCQ.md`](08-MCQ.md) — Self-assessment diagnostic test with detailed explanations.
- [`09-Quick-Revision.md`](09-Quick-Revision.md) — One-pager cheat sheet, key equations, and summary comparison tables.
