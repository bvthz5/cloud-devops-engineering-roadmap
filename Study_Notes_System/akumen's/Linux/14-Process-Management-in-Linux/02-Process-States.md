# 02 - Process States

A process is not always actively executing code on the CPU. The Linux kernel constantly switches processes between the CPU and memory (context switching). At any given moment, a process is in one of several defined **states**.

Understanding these states is critical for diagnosing system performance issues (e.g., why is the system slow even though CPU usage is low? Look for processes in the 'D' state).

---

## 🚦 The 5 Primary Process States

When you view processes using tools like `ps` or `top`, the state is usually represented by a single capital letter under the `STAT` or `S` column.

| State Code | Name | Description |
| :---: | :--- | :--- |
| **R** | **Running / Runnable** | The process is either currently executing on the CPU, or it is in the run queue waiting for its turn. |
| **S** | **Interruptible Sleep** | The process is waiting for an event to complete (e.g., waiting for user input, network data, or a timer). It can be "interrupted" (woken up) by a signal. *Most processes on a healthy system are in this state.* |
| **D** | **Uninterruptible Sleep** | The process is waiting for hardware I/O (like a disk read). It **cannot** be interrupted by signals, not even `SIGKILL` (kill -9). It must wait for the hardware to respond. |
| **T** | **Stopped** | The process has been suspended (paused). This happens if you press `Ctrl+Z` in the terminal, or send a `SIGSTOP` signal. It will not execute until it receives a `SIGCONT` (continue) signal. |
| **Z** | **Zombie** | The process has finished executing and died, but its parent process hasn't yet collected its exit status. It consumes no CPU or memory, but it holds a slot in the process table. |

---

## 🧟 Deep Dive: The Zombie State (Z)

Zombies are a frequent point of confusion and a common interview topic.

**How a Zombie is Created:**
1. A child process completes its execution and calls `exit()`.
2. The child's resources (RAM, open files) are released by the kernel.
3. However, the kernel keeps a tiny record (the PID and the exit code) in the process table. It waits for the parent process to acknowledge the death by calling `wait()`.
4. While waiting for the parent to call `wait()`, the child is a **Zombie (Z)**.

**Are Zombies dangerous?**
A few zombies are harmless. They consume almost zero resources. However, if a badly written parent process continually spawns children and never calls `wait()`, the process table will eventually fill up. Once all available PIDs are consumed, the system cannot start any new processes, requiring a reboot.

**How to kill a Zombie?**
You **cannot** kill a zombie using `kill -9` because it is already dead. 
To clear a zombie, you must either:
1. Fix the parent process so it properly calls `wait()`.
2. Kill the *parent* process. When the parent dies, the zombie is adopted by `systemd` (PID 1), which immediately calls `wait()` and reaps the zombie.

---

## 💽 Deep Dive: Uninterruptible Sleep (D)

A process in the `D` state is waiting directly on the kernel/hardware, usually for disk or network storage (like NFS) to respond.

**Why is it uninterruptible?**
If the kernel allowed you to kill a process in the middle of a delicate hardware interaction, it could cause data corruption or leave hardware in an inconsistent state.

**The Danger of the 'D' State:**
Processes in the `D` state contribute to the system's **Load Average**. If a hard drive is failing, or an NFS mount drops off the network, processes trying to read/write to it will get stuck in the `D` state indefinitely. The load average will skyrocket, and the system may become unresponsive, even if CPU usage is 0%.

You cannot kill a `D` state process. You must fix the underlying hardware/storage issue, or reboot the server.

---

## ➕ Additional State Modifiers (BSD syntax)

In the output of `ps aux`, you often see additional characters appended to the primary state letter (e.g., `Ss`, `R+`, `S<`).

*   `s` = is a session leader (usually a shell like bash).
*   `+` = is in the foreground process group (actively attached to a terminal).
*   `<` = is high priority (not nice to other processes).
*   `N` = is low priority (nice to other processes).
*   `l` = is multi-threaded.
