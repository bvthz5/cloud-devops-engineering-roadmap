# Curriculum Mapping & Source Attribution

This module expands the **Linux Kernel & Distributions** foundational syllabus into an enterprise-grade reference for Cloud & DevOps engineers.

---

## 📖 Source-Derived Foundation (Supplied Material)

The core concepts directly derived from the supplied curriculum include:
1. **The Kernel Definition:**
   - The core component of the operating system.
   - The authoritative bridge between application software and physical hardware.
2. **The 4 Primary Kernel Responsibilities:**
   - **Process Management:** Creating, executing, scheduling, and destroying processes.
   - **Memory Management:** Allocating, isolating, and freeing physical RAM and virtual address spaces.
   - **Filesystem Management:** Organizing files into structured directories, managing blocks, and reading/writing data.
   - **Device Control:** Communicating with hardware peripherals via device drivers and interrupt handling.
3. **Distribution Fundamentals:**
   - The distribution as a complete package (Kernel + Package Manager + GNU Tools + Desktop/Server environment).
   - Core differentiators: Package managers (`apt`, `dnf`, `pacman`, `apk`), Desktop Environments, Release Models (Fixed LTS vs. Rolling), and Target Audiences.

---

## 🚀 Extended Cloud & DevOps Additions (Beyond the Core Syllabus)

The foundational principles were systematically broadened into a comprehensive 18-part study system:

**Kernel Basics ➔ Core Responsibilities ➔ User vs. Kernel Space ➔ System Calls ➔ Process Subsystem (CFS) ➔ Memory Subsystem (Buddy/SLUB) ➔ VFS Storage ➔ Device Drivers & LKMs ➔ Kernel Architecture ➔ Kernel CLI Commands ➔ Linux Distributions ➔ Production Scenarios ➔ Troubleshooting ➔ Interview Q&A ➔ Hands-on Labs ➔ MCQs ➔ Quick Revision ➔ Related Topics**

Key architectural extensions include:
1. **Kernel Privilege Architecture:**
   - CPU Ring 0 (Supervisor) vs. Ring 3 (User Mode), virtual memory address partitioning (lower 128 TB user vs. upper 128 TB kernel), and Kernel Page Table Isolation (KPTI).
2. **Subsystem Mechanics:**
   - **Scheduler:** Completely Fair Scheduler (CFS) with red-black trees ordered by `vruntime` and the modern EEVDF scheduler.
   - **Memory:** Buddy Allocator (physical pages) coupled with the SLUB allocator (small kernel objects) to avoid internal fragmentation.
   - **VFS:** Superblocks, Inodes, Dentries (`dcache`), and modern block I/O schedulers (`none` for NVMe).
   - **Drivers & LKMs:** Dynamic module injection (`modprobe`, `insmod`), major/minor device numbers, and `modules.dep`.
3. **Modern Kernel Innovations:**
   - **eBPF (Extended Berkeley Packet Filter):** In-kernel verifier, JIT compilation, and runtime observability (Cilium, Falco, BCC).
4. **SRE Incident Response Playbooks:**
   - Soft lockup watchdog thresholds and automated reboot flags (`kernel.softlockup_panic`).
   - Diagnosing "Phantom" Kernel SLAB memory leaks using `slabtop`.
   - Kernel Live Patching (`kpatch` / Livepatch) to secure production nodes without reboots.
   - Analyzing hung tasks and extracting stack traces from `/proc/[PID]/stack`.
5. **Practical Validation:**
   - 10 Technical interview questions with deep architectural answers.
   - 4 Terminal labs (`sysctl` live tuning, `slabtop` cache tracking, `modprobe` dependency trees, and `dmesg` filtering).
   - 10 Self-assessment multiple-choice questions with detailed explanations.
   - 5-Minute pre-interview revision cheat sheet.
