# SOURCE: Core Components of a Linux Machine

This document outlines the origin, foundational inputs, and structural expansions applied to create the **07-Core-Components-of-a-Linux-Machine** topic within the **Akumen Study System**.

---

## 📌 Primary Inputs & Scope

The foundation of this topic is built upon the classic layered architecture model of a Linux host:

1. **Hardware Layer:** Physical CPU, RAM, NVMe/SATA storage, NICs, PCIe bus.
2. **Linux Kernel & Device Drivers:** Core kernel subsystems (process scheduling, virtual memory, VFS, networking) and loadable kernel modules (LKMs).
3. **System Libraries (`glibc`):** GNU C library system call wrappers and dynamic linking.
4. **System Utilities & Binaries:** Core utilities (`ls`, `cat`, `grep`, `ps`, `systemctl`).
5. **The Shell:** Command-line interpreters (`bash`, `zsh`) and execution loops.
6. **User Applications & Services:** Interactive user tools, background daemons, and containerized runtimes.

---

## 🛠️ Educational Expansions & DevOps Extensions

To transform high-level component diagrams into a production-grade DevOps study guide, the original prompt material was systematically expanded to include:

- **Hardware Ring Protection Model:** Explanation of Ring 0 (Kernel Space) vs Ring 3 (User Space).
- **Practical `cd` vs `ls` Distinction:** In-depth mechanics of why `cd` must be a shell built-in to modify parent shell state while `ls` executes as an external binary file (`/usr/bin/ls`).
- **End-to-End Command Traversal Flows:** Step-by-step trace of `cat /var/log/syslog` and incoming network packet processing across all 6 layers.
- **Dynamic vs Static Linking:** Detailed comparison of `.so` vs `.a` linking and troubleshooting `GLIBC` version mismatch errors in cloud deployments.
- **Assessment Suite:** Technical interview questions, hands-on terminal labs, multiple-choice questions, and quick revision cheat sheets.

---

## 📂 Topic File Index

- [`README.md`](./README.md)
- [`01-Layered-Architecture-Overview.md`](./01-Layered-Architecture-Overview.md)
- [`02-Hardware-Layer.md`](./02-Hardware-Layer.md)
- [`03-Linux-Kernel-Core.md`](./03-Linux-Kernel-Core.md)
- [`04-Device-Drivers.md`](./04-Device-Drivers.md)
- [`05-System-Libraries-and-Glibc.md`](./05-System-Libraries-and-Glibc.md)
- [`06-System-Utilities-and-Binaries.md`](./06-System-Utilities-and-Binaries.md)
- [`07-The-Shell.md`](./07-The-Shell.md)
- [`08-User-Applications-and-Services.md`](./08-User-Applications-and-Services.md)
- [`09-Command-Flow-ls-cat-networking.md`](./09-Command-Flow-ls-cat-networking.md)
- [`10-Shell-Builtins-vs-External-Binaries-cd-vs-ls.md`](./10-Shell-Builtins-vs-External-Binaries-cd-vs-ls.md)
- [`11-Practical-Commands.md`](./11-Practical-Commands.md)
- [`12-Real-World-Scenarios.md`](./12-Real-World-Scenarios.md)
- [`13-Troubleshooting.md`](./13-Troubleshooting.md)
- [`14-Interview-QA.md`](./14-Interview-QA.md)
- [`15-Hands-On-Practice.md`](./15-Hands-On-Practice.md)
- [`16-MCQ.md`](./16-MCQ.md)
- [`17-Quick-Revision.md`](./17-Quick-Revision.md)
- [`18-Related-Topics.md`](./18-Related-Topics.md)
---

| Back to Index |
| :---: |
| [README](./README.md) |
