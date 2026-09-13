# Curriculum Mapping & Source Attribution

This module establishes the core **Operating System** foundations for the Akumen study system, building directly upon the introductory course materials and expanding them into enterprise Cloud & DevOps applications.

---

## 📖 Source-Derived Foundation (Supplied Material)

The foundational framework of this module directly mirrors the supplied course notes:
1. **The 4-Tier Abstraction Model:**
   ```text
   User
     ↓
   Application
     ↓
   Operating System
     ↓
   Hardware
   ```
2. **Primary Operating System Responsibilities:**
   - **Processor / CPU Management:** Arbitrating and scheduling compute tasks.
   - **Memory Management:** Allocating, tracking, and freeing RAM.
   - **File Management:** Organizing storage structures, directories, and persistence.
   - **Device & I/O Coordination:** Bridging communications between software and hardware peripherals.

---

## 🚀 Extended Cloud & DevOps Additions (Beyond the Core Syllabus)

To make these operating systems concepts directly applicable to senior engineering, SRE, and cloud operations, the foundation was expanded across the complete learning chain:

**OS ➔ Kernel ➔ User Space ➔ System Calls ➔ Processes ➔ Memory ➔ Filesystems ➔ Devices ➔ Networking ➔ Security ➔ Linux Distributions ➔ Troubleshooting ➔ DevOps**

Key added components include:
1. **Process & CPU Deep Dive:**
   - 5-State process lifecycle with Linux state codes (`R`, `S`, `D`, `T`, `Z`).
   - Completely Fair Scheduler (CFS) mechanics and red-black trees.
   - Zombie vs. Orphan process mechanics and PID 1 adoption.
2. **Virtual Memory & Paging Mechanics:**
   - Paging, frames, MMU address translation, and the Translation Lookaside Buffer (TLB).
   - Step-by-step page fault handling flow and page replacement algorithms.
   - Demystifying memory metrics: **VSZ** vs. **RSS**.
3. **Historical Evolution & Distribution Landscape:**
   - 1969 Bell Labs UNIX, C language creation, the GNU Project, and the 1991 Linux release.
   - Enterprise distribution matrix: Debian/Ubuntu vs. RHEL/Rocky/Alma vs. Alpine vs. Immutable Cloud OS (Talos/Flatcar).
4. **Real-World DevOps Incidents:**
   - Deadlocks and the 4 Coffman conditions.
   - Kubernetes container CPU throttling via CFS period and quota.
   - SRE triage playbooks: High load average with low CPU, memory thrashing, and file descriptor limits.
5. **Practical Validation:**
   - 10 Technical interview questions with structured answers.
   - 4 Hands-on terminal labs (`pstree`, minor/major page faults, zombie reproduction, and hardware interrupts).
   - 10 Multiple choice self-assessment questions.
   - 5-Minute pre-interview revision cheat sheet.
