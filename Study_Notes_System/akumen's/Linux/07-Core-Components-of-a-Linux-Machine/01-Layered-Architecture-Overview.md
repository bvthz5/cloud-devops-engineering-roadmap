# 01 - Layered Architecture Overview of a Linux Machine

A Linux machine is fundamentally designed as a stack of modular software and hardware abstractions. Each layer relies on the service provided by the layer immediately below it.

---

## 🏛️ The 6 Architectural Layers

```text
+-------------------------------------------------------------+
| 6. User Applications (Nginx, Docker, Web Browsers, Python)  |  USER SPACE
| 5. Shell Interpreters (Bash, Zsh, Sh, Fish)                 |  (Ring 3)
| 4. System Utilities (ls, cat, grep, ps, top, systemctl)     |  Unprivileged
| 3. System Libraries (glibc, libssl, libpthread, libm)       |  Execution
+-------------------------------------------------------------+
================ SYSTEM CALL INTERFACE (syscall) =================
+-------------------------------------------------------------+
| 2. Linux Kernel & Device Drivers (Core OS, VFS, Drivers)    |  KERNEL SPACE
+-------------------------------------------------------------+  (Ring 0)
================ HARDWARE INTERACTION (Bus / IRQ) ===============  Privileged
+-------------------------------------------------------------+
| 1. Hardware Layer (CPU, RAM, Disks, NICs, Motherboard)      |
+-------------------------------------------------------------+
```

---

## 🔒 User Space vs. Kernel Space Isolation

To prevent user programs from corrupting hardware or crashing the entire system, CPU hardware architectures (such as x86_64 and ARM64) enforce execution protection rings:

- **Ring 0 (Kernel Space):** Privileged mode. The Linux Kernel executes here with complete, unrestricted access to physical CPU registers, memory control blocks, and device I/O ports.
- **Ring 3 (User Space):** Unprivileged mode. All user applications, shells, system utilities, and libraries execute here. Direct access to hardware registers or memory belonging to other processes is forbidden by the hardware MMU (Memory Management Unit).

---

## 🌉 The System Call Bridge

When a user utility like `cat` needs to read a file stored on an NVMe SSD, it cannot read the disk directly. Instead:
1. `cat` calls `glibc` library function `fopen()` / `fread()`.
2. `glibc` executes a CPU trap instruction (`syscall` on x86_64), passing the system call number for `read()`.
3. The CPU switches hardware context from **Ring 3** to **Ring 0**.
4. The Kernel processes the `read()` request via its Virtual Filesystem (VFS) and device driver.
5. The Kernel copies data to user-space memory and switches the CPU back to **Ring 3**.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Hardware Layer](./02-Hardware-Layer.md) |
