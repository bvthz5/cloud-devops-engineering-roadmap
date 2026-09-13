# 11 - Practical Commands for Component Inspection

Essential command-line tools for inspecting components across hardware, kernel drivers, system libraries, and process execution layers.

---

## 🛠️ Layer Inspection Command Reference

| Target Layer | Tool / Command | Primary Function & Usage Example |
| :--- | :--- | :--- |
| **Hardware** | `lscpu`, `free`, `lspci`, `lsusb` | Hardware topology inspection (`lscpu`, `lspci -v`). |
| **Kernel Core** | `uname`, `dmesg`, `/proc` | View kernel version (`uname -r`), kernel ring buffer logs (`dmesg -T \| tail`). |
| **Device Drivers** | `lsmod`, `modprobe`, `modinfo` | List active kernel modules (`lsmod`), view module details (`modinfo e1000e`). |
| **System Libraries** | `ldd`, `ldconfig`, `nm` | List shared library dependencies (`ldd /usr/bin/python3`). |
| **System Utilities** | `type`, `which`, `whereis` | Identify command classification (`type cd`, `which grep`). |
| **Process / System Calls** | `strace`, `ltrace`, `ps`, `top` | Trace system calls of a process (`strace -p <PID>` or `strace ls`). |

---

## 💻 Practical Inspection Recipes

```bash
# 1. Trace real-time system calls executed by 'cat /etc/hostname'
strace cat /etc/hostname

# 2. Check glibc version installed on host
ldd --version | head -n 1

# 3. View hardware PCIe device tree with kernel driver in use
lspci -k | head -n 20

# 4. View kernel ring buffer boot messages related to memory or storage
dmesg -T | grep -iE 'memory|nvme|sda' | head -n 15
```

---

## ⬅️ Navigation
- Previous: [10 - Shell Built-ins vs External Binaries](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/10-Shell-Builtins-vs-External-Binaries-cd-vs-ls.md)
- Next: [12 - Real-World Production Scenarios](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/12-Real-World-Scenarios.md)
