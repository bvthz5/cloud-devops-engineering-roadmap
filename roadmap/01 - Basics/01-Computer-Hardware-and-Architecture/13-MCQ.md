# 13 — Multiple Choice Questions (Self-Assessment)

Test your mastery of Computer Hardware and Architecture. Each question contains four options and an expandable answer with deep technical explanation.

---

### Q1: What is the primary function of the Program Counter (PC) register in a CPU?
- **A)** Storing the result of the most recent arithmetic calculation performed by the ALU.
- **B)** Holding the memory address of the next instruction waiting to be fetched and executed.
- **C)** Maintaining the base address of the call stack for active functions.
- **D)** Caching the most recently accessed page table entry from physical memory.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The Program Counter (PC)—called `RIP` in x86_64 and `PC` in ARM—holds the memory address of the next sequential machine instruction to execute. Once an instruction is fetched, the PC automatically increments, unless modified by branch, jump, or call instructions.
</details>

---

### Q2: Why is the x86-64 architecture classified as CISC while ARM64 is classified as RISC?
- **A)** x86-64 uses fixed 32-bit instructions; ARM64 uses variable-length instructions from 1 to 15 bytes.
- **B)** x86-64 requires all operations to load data into registers first; ARM64 allows memory-to-memory arithmetic.
- **C)** x86-64 provides complex, variable-length instructions that can operate directly on memory; ARM64 uses simple, fixed-length 32-bit instructions with a strict load/store architecture.
- **D)** x86-64 can only run 32-bit operating systems; ARM64 is strictly a 64-bit architecture.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** CISC (Complex Instruction Set Computer) like x86 offers rich, multi-operation instructions that can directly access memory addresses within instructions. RISC (Reduced Instruction Set Computer) like ARM simplifies instructions into uniform, fixed lengths and restricts all arithmetic operations strictly to registers, accessing memory solely through dedicated `LOAD` and `STORE` operations.
</details>

---

### Q3: When a CPU encounters a Cache Miss, from which tier is the requested data fetched if it is absent in L1, L2, and L3 caches?
- **A)** CPU General Purpose Registers
- **B)** Main Memory (DRAM)
- **C)** NVMe PCIe Storage
- **D)** Translation Lookaside Buffer (TLB)

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The CPU cache hierarchy proceeds sequentially: L1 → L2 → L3. If the data is absent from all cache levels (a complete cache miss), the hardware memory controller issues a read request to Main Memory (DRAM), incurring a ~50-100ns latency penalty before caching the line and delivering it to CPU registers.
</details>

---

### Q4: Which component performs the real-time translation of Virtual Addresses into Physical Addresses?
- **A)** Control Unit (CU)
- **B)** Memory Management Unit (MMU)
- **C)** Arithmetic Logic Unit (ALU)
- **D)** Direct Memory Access Controller (DMAC)

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The MMU (Memory Management Unit) is a hardware circuit in the CPU that dynamically intercepts virtual address requests from running programs and translates them into physical RAM addresses using page tables and the TLB.
</details>

---

### Q5: What is a Major Page Fault in an operating system?
- **A)** An unrecoverable hardware failure in the physical DRAM chip resulting in a kernel panic.
- **B)** An access violation where a process attempts to write to a read-only memory segment, triggering a segmentation fault.
- **C)** A page fault where the requested memory page is not present in physical RAM and must be loaded from secondary storage (disk or swap).
- **D)** A cache miss occurring inside the Translation Lookaside Buffer (TLB).

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** A Minor Page Fault occurs when the page is in RAM but not yet mapped in the process's page table. A Major Page Fault requires the kernel to pause execution and issue synchronous disk I/O to read the page from disk or swap space, incurring a massive latency penalty (several milliseconds).
</details>

---

### Q6: Why do NVMe SSDs achieve significantly higher IOPS than SATA SSDs?
- **A)** NVMe drives utilize DRAM chips rather than NAND flash memory.
- **B)** NVMe drives connect via SATA III cables but use higher voltage levels.
- **C)** NVMe bypasses the PCIe bus entirely, communicating directly through the CPU L3 cache.
- **D)** NVMe connects directly to PCIe lanes and supports up to 64,000 queues with 64,000 commands each, whereas SATA (AHCI) is limited to 1 queue with 32 commands.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: D**  
**Explanation:** SATA relies on the legacy AHCI protocol designed for spinning disks, limited to a single queue with 32 commands. NVMe was engineered specifically for solid-state storage over the PCIe bus, featuring up to 64,000 parallel queues with 64,000 commands per queue, enabling multi-core parallelism without lock contention.
</details>

---

### Q7: What is the primary purpose of Direct Memory Access (DMA)?
- **A)** Allowing the CPU to execute instructions directly from secondary disk storage.
- **B)** Enabling high-speed peripherals (like NICs and NVMe controllers) to transfer data directly to and from system RAM without utilizing CPU execution cycles.
- **C)** Bypassing page table protections to grant userland processes root privileges.
- **D)** Enabling CPU L1 cache to mirror physical RAM contents continuously.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** DMA offloads data transfer tasks from the CPU. Hardware devices copy buffers directly into and out of physical memory across the system bus, raising a single interrupt once the transfer is finished.
</details>

---

### Q8: In UEFI systems, what role does the EFI System Partition (ESP) play?
- **A)** It stores the virtual swap file used when physical RAM is exhausted.
- **B)** It is a FAT32-formatted partition containing UEFI-executable bootloader binaries (`.efi` files) and drivers.
- **C)** It holds the encrypted master password for hardware-level drive encryption.
- **D)** It serves as an unformatted raw block storage area containing the 512-byte Master Boot Record.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** UEFI firmware reads standard filesystems, specifically FAT12/16/32. The EFI System Partition (ESP) contains bootloader executables (such as `BOOTX64.EFI` or `grubx64.efi`) and kernel stubs that the UEFI firmware can execute directly.
</details>

---

### Q9: Which of the following is an example of a Type 1 Hypervisor?
- **A)** Oracle VirtualBox
- **B)** VMware Workstation
- **C)** VMware ESXi
- **D)** Docker Desktop

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** VMware ESXi, KVM (in Linux), and Xen are Type 1 (bare-metal) hypervisors that run directly on physical server hardware. VirtualBox and VMware Workstation are Type 2 (hosted) hypervisors running on top of a guest operating system. Docker is a container runtime, not a hypervisor.
</details>

---

### Q10: What metric indicates that a cloud virtual machine is being throttled by the underlying hypervisor due to host CPU contention or credit exhaustion?
- **A)** High `%usr` (User CPU percentage)
- **B)** High `%sys` (System/Kernel CPU percentage)
- **C)** High `%st` (CPU Steal time percentage)
- **D)** High `%wa` (I/O Wait time percentage)

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** CPU Steal Time (`%st`) measures the percentage of time a virtual CPU had runnable threads ready for execution but was prevented from running by the hypervisor because the physical CPU was servicing other virtual machines or the VM exhausted its burst credits.
</details>

---

### Q11: What is the main architectural distinction between a Virtual Machine and a Container?
- **A)** VMs isolate network packets; containers isolate memory pages only.
- **B)** VMs package a complete guest OS with a virtualized hardware layer via a hypervisor; containers share the host OS kernel and isolate processes using kernel namespaces and cgroups.
- **C)** Containers require dedicated hardware CPU cores; VMs share cores via software emulation.
- **D)** Containers run in Kernel Space; Virtual Machines run exclusively in User Space.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** A VM bundles a full operating system kernel, device drivers, and simulated virtual hardware managed by a hypervisor. A container is a standard Linux process that shares the host kernel directly, using namespaces for isolation and cgroups for resource constraints.
</details>

---

### Q12: Why are Huge Pages (e.g., 2 MB or 1 GB) used in memory-intensive databases like Redis or PostgreSQL?
- **A)** They increase the physical clock speed of the DDR memory bus.
- **B)** They compress data in RAM using hardware acceleration algorithms.
- **C)** They reduce the number of entries in the Translation Lookaside Buffer (TLB), drastically lowering TLB misses during random memory lookups.
- **D)** They allow the database to bypass virtual memory and access raw physical addresses directly.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Standard page size is 4 KB. Managing 256 GB of memory requires 67 million page entries, causing frequent TLB misses. Using 2 MB Huge Pages reduces the number of entries by a factor of 512, keeping the active address mappings inside the high-speed hardware TLB cache.
</details>
