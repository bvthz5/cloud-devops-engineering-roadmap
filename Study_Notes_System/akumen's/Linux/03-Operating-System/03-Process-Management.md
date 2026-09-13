# 03 - Process Management & CPU Scheduling

A **process** is a program in execution. While a program is a passive entity stored on disk (an executable file like `/bin/bash`), a process is an active entity with a program counter, memory registers, stack, and assigned resources.

---

## 1. Process vs. Thread

```
┌─────────────────────────────────────────────────────────────┐
│ Process Address Space (PID: 1050)                           │
│ ┌───────────────┐ ┌───────────────┐ ┌─────────────────────┐ │
│ │  Code (Text)  │ │ Global (Data) │ │ Heap (malloc/mmap)  │ │
│ └───────────────┘ └───────────────┘ └─────────────────────┘ │
│                                                             │
│   Thread 1 (TID 1050)      Thread 2 (TID 1051)              │
│   ┌─────────────────┐      ┌─────────────────┐              │
│   │ Registers / PC  │      │ Registers / PC  │              │
│   │ Dedicated Stack │      │ Dedicated Stack │              │
│   └─────────────────┘      └─────────────────┘              │
└─────────────────────────────────────────────────────────────┘
```

- **Process:** An isolated unit of execution with its own virtual memory space, file descriptor table, and security credentials.
- **Thread:** A lightweight unit of execution within a process. Multiple threads share the same address space, global variables, and open files, but maintain their own instruction pointer (PC), registers, and stack.

---

## 2. Process Control Block (PCB) & `task_struct`

To manage each process, the operating system kernel maintains a data structure called the **Process Control Block (PCB)**. In Linux, this is the `task_struct`:

- **Process Identifier (PID):** Unique integer identifying the task.
- **Process State:** Running, Ready, Waiting, Zombie, etc.
- **CPU Registers & Program Counter:** Saved state when the process is preempted.
- **Scheduling Information:** Priority (`nice` value `-20` to `+19`), policy, virtual runtime (`vruntime`).
- **Memory Management Info:** Pointers to page tables (`mm_struct`).
- **Accounting & Timers:** CPU time consumed, user time, system time.
- **File Descriptor Table:** Pointers to open files (`files_struct`).

---

## 3. The 5-State Process Model

```
                    ┌───────────────┐
                    │      NEW      │
                    └───────┬───────┘
                            │ Admitted
                            ▼
              ┌───────────────────────────┐
              │           READY           │◄────────────┐
              │ (Waiting in run queue)    │             │
              └─────────────┬─────────────┘             │
                            │ Dispatched / Scheduled    │
                            ▼                           │
              ┌───────────────────────────┐             │
    ┌─────────┤          RUNNING          ├─────────────┘
    │         │  (Executing on CPU core)  │ Time Slice Expired (Preempted)
    │         └─────────────┬─────────────┘
    │                       │
    │ I/O or Event Wait     │ Exit / Terminated
    ▼                       ▼
┌─────────────────────────┐ ┌───────────────┐
│     WAITING / BLOCKED   │ │  TERMINATED   │
│ (Waiting for disk/net)  │ │   (Zombie)    │
└───────────┬─────────────┘ └───────────────┘
            │ Event Occurred / I/O Complete
            └───────────────► (Returns to READY)
```

### Linux Process State Codes (Seen in `ps` and `top`):
- **`R` (Running / Runnable):** Currently executing on a CPU core or sitting in the ready run-queue.
- **`S` (Interruptible Sleep):** Sleeping, waiting for an event (keystroke, timer, network packet); wakes up if a signal is sent.
- **`D` (Uninterruptible Sleep):** Sleeping in a kernel device driver waiting on hardware I/O. Does not respond to signals.
- **`T` (Stopped):** Paused by a signal (e.g., `Ctrl+Z` sending `SIGTSTP` or `kill -STOP`).
- **`Z` (Zombie):** Process has completed execution (`exit()`), but its parent has not yet read its exit code via `wait()`. Consumes 0 bytes of RAM, but holds an entry in the PID table.

---

## 4. CPU Scheduling Algorithms

The CPU scheduler determines which ready process is allocated CPU execution time:

| Scheduling Algorithm | Preemptive? | Core Concept | Pros / Cons |
|---|:---:|---|---|
| **First-Come, First-Served (FCFS)** | No | Processes execute in arrival order. | Simple, but suffers from "Convoy Effect" (short jobs stuck behind long ones). |
| **Shortest Job First (SJF)** | No/Yes | Executes process with shortest CPU burst time first. | Mathematically optimal average wait time, but impossible to predict exact burst times. |
| **Round Robin (RR)** | **Yes** | Each process gets a fixed time slice (**quantum**, e.g., 10-50ms). | Fair and responsive; quantum size is critical (too small = context switch overhead). |
| **Multi-Level Feedback Queue (MLFQ)** | **Yes** | Multiple priority queues; I/O-bound jobs move up, CPU-bound jobs move down. | Balances batch jobs with interactive responsiveness. |
| **Linux CFS (Completely Fair Scheduler)** | **Yes** | Uses a self-balancing **Red-Black Tree** ordered by virtual runtime (`vruntime`). | Guarantees mathematically fair CPU share based on `nice` weight. |

---

## 5. Process Lifecycle: Zombies vs. Orphans

```
┌──────────────────────────────────────────────────────────┐
│                   THE ORPHAN PROCESS                     │
│ 1. Parent process crashes or exits.                      │
│ 2. Child process is still running.                       │
│ 3. Orphan child is immediately adopted by PID 1          │
│    (systemd), which supervises it and reaps it.          │
├──────────────────────────────────────────────────────────┤
│                   THE ZOMBIE PROCESS                     │
│ 1. Child process calls exit(0) and terminates.           │
│ 2. Kernel frees memory/files, but retains PID & exit code│
│    in PCB so parent can read it.                         │
│ 3. Parent fails or forgets to call wait() / waitpid().   │
│ 4. Child becomes a "Zombie" (defunct).                   │
│ 5. Fix: You cannot kill a zombie (it is already dead).   │
│    Kill or restart the negligent PARENT process!         │
└──────────────────────────────────────────────────────────┘
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Functions of OS](./02-Functions-of-OS.md) | [README](./README.md) | [04 - Memory Management](./04-Memory-Management.md) |
