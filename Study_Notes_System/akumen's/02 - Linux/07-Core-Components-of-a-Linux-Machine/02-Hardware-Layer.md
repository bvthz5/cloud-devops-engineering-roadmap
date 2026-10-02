# 02 - The Hardware Layer

The bottom layer of a Linux machine consists of the physical hardware components that execute electronic instructions, store bit data, and manage network signaling.

---

## ⚡ Core Hardware Components

| Hardware Subsystem | Component Function | Linux Interaction Interface |
| :--- | :--- | :--- |
| **CPU (Central Processing Unit)** | Fetches, decodes, and executes instructions. Enforces privilege rings (Ring 0 vs Ring 3). | `/proc/cpuinfo`, `lscpu` |
| **RAM (Random Access Memory)** | Volatile high-speed storage holding active process code, stack/heap memory, and kernel buffers. | `/proc/meminfo`, `free -h` |
| **Storage Controllers & Drives** | Persistent storage (NVMe SSDs, SATA drives, RAID controllers). | `/sys/block/`, `lsblk`, `fdisk` |
| **Network Interface Card (NIC)** | Physical Ethernet/Wi-Fi chips handling packet transmission and receipt at Layer 1/2. | `/sys/class/net/`, `ip link` |
| **Bus Architectures (PCIe, USB)** | Physical communication pathways connecting peripheral devices to the CPU and RAM. | `lspci`, `lsusb` |

---

## 🔔 Interrupt Requests (IRQs)

When hardware receives data (such as a network packet hitting a NIC or a key press on a keyboard):
1. The device signals the CPU by triggering a hardware **Interrupt Request (IRQ)** line.
2. The CPU temporarily pauses the currently executing user process.
3. The CPU jumps execution to the kernel's registered **Interrupt Handler** function for that specific hardware device.
4. Once processed, execution resumes where the user application left off.

```bash
# View real-time hardware interrupt counts per CPU core
cat /proc/interrupts
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Layered Architecture Overview](./01-Layered-Architecture-Overview.md) | [README](./README.md) | [03 - Linux Kernel Core](./03-Linux-Kernel-Core.md) |
