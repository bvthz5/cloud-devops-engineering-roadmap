# Master Glossary, Common Mistakes & Completion Checklist

## 1. Essential Vocabulary Glossary

### Hardware
- **CPU (Central Processing Unit):** The brain of the computer that fetches, decodes, and executes instructions.
- **ALU (Arithmetic Logic Unit):** CPU subsystem performing mathematical and logical comparisons.
- **Control Unit (CU):** Directs signal flow and instruction cycles within the processor.
- **Registers:** Fast, small storage slots directly inside the processor core.
- **Cache (L1/L2/L3):** High-speed SRAM buffers mitigating DRAM memory latency.
- **RAM (Random Access Memory):** Volatile primary memory holding running programs and OS structures.
- **NVMe:** Protocol for ultra-fast SSD storage operating directly over the PCIe bus.
- **Hypervisor:** Software/firmware initializing and managing virtual machine instances.

### Operating System & CLI
- **Kernel:** Privileged core of the OS handling memory, processes, I/O, and hardware.
- **User Space vs. Kernel Space:** Isolation boundary separating unprivileged apps (Ring 3) from privileged kernel ops (Ring 0).
- **System Call (Syscall):** Controlled API gateway for user processes to request kernel services.
- **Process:** An isolated running instance of a program with virtual memory and file descriptors.
- **Thread:** Concurrent execution line inside a process sharing its address space.
- **File Descriptor (FD):** Integer key representing an open file, socket, or pipe (`0` stdin, `1` stdout, `2` stderr).
- **Shell:** Command interpreter parsing text commands (`bash`, `zsh`).
- **Terminal Emulator:** GUI application window hosting a shell session.

---

## 2. Common Beginner Pitfalls to Avoid

1. Confusing CPU clock speed (GHz) with overall multi-threaded performance.
2. Confusing volatile RAM working memory with persistent storage (SSD/NVMe).
3. Conflating a terminal emulator (e.g., Windows Terminal) with the shell (`bash` or `powershell`).
4. Assuming every command is a standalone binary file (forgetting shell builtins and aliases).
5. Ignoring command exit codes (`$?`) in automated scripts.
6. Using tabs instead of spaces in YAML files.
7. Committing API keys, passwords, or SSH keys directly to Git repositories.
8. Assuming a process is dead without checking if it became a Zombie process.
9. Editing shell scripts on Windows without converting CRLF line endings to Linux LF (`dos2unix`).

---

## 3. Foundation Readiness Checklist

Mark these checkboxes once you can confidently explain each topic:

- [x] CPU execution cycle (Fetch-Decode-Execute)
- [x] x86_64 vs ARM64 architecture compatibility
- [x] RAM vs SSD vs NVMe performance tiers
- [x] Hypervisors (Type 1 vs Type 2) vs Containers
- [x] User Space (Ring 3) vs Kernel Space (Ring 0)
- [x] System Call mechanics (`fork`, `execve`, `open`, `read`, `write`)
- [x] Process lifecycle & PID 1 responsibilities
- [x] Standard file descriptors (`0` stdin, `1` stdout, `2` stderr)
- [x] Terminal Emulator vs Shell vs Command
- [x] `PATH` variable executable resolution
- [x] Exit status codes (`0` vs `non-zero`)
- [x] Pipelines (`|`) and Redirection (`>`, `>>`, `2>&1`)
- [x] YAML syntax, indentation, and list rules
- [x] JSON data types and syntax strictness
- [x] System signals (`SIGTERM` 15 vs `SIGKILL` 9)
- [x] Bottom-up layer-based troubleshooting methodology
