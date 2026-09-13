# 05 - Kernel Subsystem: Process Management & Scheduling

Process management is the kernel subsystem responsible for allocating CPU execution time, switching tasks, maintaining process state, and providing container isolation primitives.

---

## 1. The Kernel's Process Representation: `struct task_struct`

In the Linux kernel source code (`<linux/sched.h>`), every running thread and process in the system is represented by an enormous C data structure called **`task_struct`** (often called the Process Descriptor).

```c
struct task_struct {
    volatile long state;               // -1 unrunnable, 0 runnable, >0 stopped
    void *stack;                       // Pointer to the process's kernel-mode stack
    pid_t pid;                         // Process ID (Thread ID from user perspective)
    pid_t tgid;                        // Thread Group ID (Main Process ID)
    struct task_struct *parent;        // Pointer to parent process
    struct list_head children;         // List of child processes
    struct mm_struct *mm;              // Virtual memory descriptors & page tables
    struct files_struct *files;        // Open file descriptor table
    struct sched_entity se;            // Scheduling entity for CFS (vruntime)
    struct nsproxy *nsproxy;           // Namespaces (Mount, Net, PID, IPC, UTS)
    // ... hundreds of other attributes
};
```

> [!IMPORTANT]
> **In Linux, Threads ARE Processes!**
> Unlike Windows or Solaris, which have completely distinct kernel architectures for threads vs processes, the Linux kernel does not distinguish between them.
> To the Linux kernel, a thread is simply a `task_struct` created with `clone()` flags sharing memory (`CLONE_VM`), files (`CLONE_FILES`), and signal handlers (`CLONE_SIGHAND`) with its parent. They are collectively called **Tasks**.

---

## 2. The CPU Scheduler: CFS & EEVDF

The Linux kernel does not use simple Round Robin scheduling. It uses sophisticated scheduling algorithms designed for multi-core scalability:

```
                            Red-Black Tree
                              (CFS Tree)
                                 [15ms]
                                /      \
                       [8ms]               [24ms]
                      /     \
              [2ms]             [12ms]
           (Smallest vruntime:
            Next to run on CPU!)
```

### The Completely Fair Scheduler (CFS):
- CFS allocates CPU time based on **`vruntime` (Virtual Runtime)**: the amount of CPU time a task has consumed, scaled by its priority weight.
- All runnable tasks are stored in a self-balancing **Red-Black Tree** ordered by `vruntime`.
- When a CPU core becomes free, the scheduler picks the leftmost node (the task that has received the least CPU time) and runs it.
- **`nice` Values (`-20` to `+19`):** A lower nice value (e.g. `-20`) increases the task's weight, causing its `vruntime` to accumulate more slowly, thereby granting it significantly more physical CPU time.

### The Modern EEVDF Scheduler (Linux 6.6+):
In Linux 6.6, Linus Torvalds and Peter Zijlstra merged **EEVDF (Earliest Eligible Virtual Deadline First)** to replace traditional CFS, providing dramatically lower latency guarantees for interactive and audio workloads.

---

## 3. Container Isolation Primitives in the Kernel

Docker, containerd, and Kubernetes do not exist as physical objects in the Linux kernel. They are constructed out of two native kernel subsystems:

```
┌─────────────────────────────────────────────────────────────┐
│                 HOW CONTAINERS WORK IN THE KERNEL           │
├──────────────────────────────┬──────────────────────────────┤
│ 1. Linux Namespaces          │ 2. Control Groups (cgroups)  │
│    (What a process can SEE)  │    (What a process can USE)  │
├──────────────────────────────┼──────────────────────────────┤
│ • PID: Isolated process list │ • CPU: Limit to 2.0 cores    │
│ • NET: Isolated IP / routing │ • Memory: Limit to 4 GB RAM  │
│ • MNT: Isolated rootfs tree  │ • I/O: Limit disk read/write │
│ • IPC: Isolated shared memory│ • PIDs: Limit max processes  │
│ • UTS: Isolated hostname     │                              │
└──────────────────────────────┴──────────────────────────────┘
```
