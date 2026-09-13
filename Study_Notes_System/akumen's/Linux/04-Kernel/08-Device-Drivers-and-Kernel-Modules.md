# 08 - Device Drivers & Loadable Kernel Modules (LKMs)

Hardware peripherals speak hundreds of proprietary bus protocols and register formats. **Device drivers** provide standardized software wrappers, while **Loadable Kernel Modules (LKMs)** allow the kernel to extend its functionality dynamically.

---

## 1. What is a Device Driver?

A **device driver** is a specialized software component running inside Kernel Space (Ring 0) that directly manipulates the electrical hardware registers, memory-mapped I/O (MMIO), and interrupt lines of a specific physical device.

```
User Space Applications (cat, curl, ffmpeg)
                    │
                    ▼ System Calls (read, write, ioctl)
┌─────────────────────────────────────────────────────────────┐
│                 Linux Kernel Subsystems                     │
│                (VFS, Sockets, Block Layer)                  │
└───────────────────────────┬─────────────────────────────────┘
                            │ Standardized Driver API
                            ▼
┌─────────────────────────────────────────────────────────────┐
│             Device Driver (e.g. e1000e.ko / nvme.ko)         │
│ • Programs hardware registers                               │
│ • Configures Direct Memory Access (DMA)                     │
│ • Handles hardware interrupts (IRQs)                        │
└───────────────────────────┬─────────────────────────────────┘
                            │ Electrical Signals over PCIe / USB
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Physical Device Controller (Intel NIC, Samsung SSD, GPU)    │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. The 3 Device Types & `/dev` Major/Minor Numbers

Every device node in `/dev` has a file type and two numbers: a **Major Number** and a **Minor Number**.

```bash
$ ls -l /dev/sda1 /dev/ttyS0
brw-rw---- 1 root disk 8,  1 Sep 13 10:00 /dev/sda1
crw-rw---- 1 root dial 4, 64 Sep 13 10:00 /dev/ttyS0
```

- **First Character:** `b` (Block Device), `c` (Character Device).
- **Major Number (`8` or `4`):** Identifies the specific **device driver** in the kernel responsible for handling I/O. (Major 8 = SCSI/SATA disk driver; Major 4 = Serial TTY driver).
- **Minor Number (`1` or `64`):** Identifies the **specific physical partition or port** managed by that driver (Minor 1 = partition 1 on disk a; Minor 64 = COM1 serial port).

---

## 3. Loadable Kernel Modules (LKMs)

A **Loadable Kernel Module (LKM)** is an object file (with the **`.ko`** extension) that can be dynamically inserted into or removed from the running Linux kernel in RAM without rebooting the system.

### Anatomy of a Simple Kernel Module in C:
```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("DevOps Engineer");
MODULE_DESCRIPTION("A Simple Linux Kernel Module");

static int __init my_module_init(void) {
    printk(KERN_INFO "MyModule: Loaded into Kernel Space (Ring 0)!\n");
    return 0;
}

static void __exit my_module_exit(void) {
    printk(KERN_INFO "MyModule: Unloaded from Kernel Space.\n");
}

module_init(my_module_init);
module_exit(my_module_exit);
```

### Essential LKM Management Commands:
```bash
# 1. List all loaded modules, size, and dependent processes:
$ lsmod

# 2. View driver information, parameters, and author:
$ modinfo overlay

# 3. Load module with all its dependencies automatically:
$ sudo modprobe overlay

# 4. Safely unload module and unused dependencies:
$ sudo modprobe -r overlay

# 5. Low-level direct loading (does not resolve dependencies):
$ sudo insmod /path/to/module.ko
$ sudo rmmod module_name
```
