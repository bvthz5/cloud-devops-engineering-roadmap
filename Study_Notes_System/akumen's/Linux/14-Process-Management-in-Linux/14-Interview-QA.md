# 14 - Interview Questions and Answers

Process management is a core competency tested heavily in DevOps and SysAdmin interviews. Expect questions focusing on state differences, signal handling, and troubleshooting methodology.

---

## 🟢 Fundamentals

**Q1. What is the difference between a program and a process?**
> A program is an executable file resting on the disk (passive). A process is an instance of that program actively executing in memory (active), possessing a PID, memory space, and state.

---

**Q2. What is PID 1 on modern Linux systems, and what is its role?**
> PID 1 is almost always `systemd` (historically `init`). It is the first process started by the kernel, responsible for booting the rest of the system, starting daemons, and adopting orphaned child processes.

---

**Q3. How do you find the PID of a running process named `nginx`?**
> You can use `pgrep nginx`, `pidof nginx`, or parse the process list manually with `ps aux | grep nginx`.

---

**Q4. What is the difference between `ps` and `top`?**
> `ps` takes a single, static snapshot of currently running processes. `top` provides a dynamic, real-time, interactive view that updates continuously (usually every 3 seconds) and highlights the most resource-intensive processes.

---

## 🔵 Applied Concepts & Signals

**Q5. Explain the difference between `SIGTERM` and `SIGKILL`.**
> `SIGTERM` (15) is a polite request to terminate. The process can catch this signal, perform cleanup operations (saving data, closing connections), and exit gracefully. This is the default signal sent by the `kill` command. 
> `SIGKILL` (9) is a forceful termination. The kernel immediately destroys the process. The process cannot catch or ignore it, meaning data loss or corruption may occur. It should only be used as a last resort.

---

**Q6. You press `Ctrl+C` in a terminal while a script is running. What signal is sent?**
> `SIGINT` (Interrupt, Signal 2). It asks the foreground job to stop.

---

**Q7. What does the `nohup` command do?**
> `nohup` (No Hangup) intercepts the `SIGHUP` signal (sent when a terminal session is closed) and ignores it. This allows a process to continue running in the background even after the user logs out. It redirects output to `nohup.out`.

---

**Q8. What is "niceness" in Linux?**
> Niceness is a value from -20 (highest priority) to +19 (lowest priority) that influences the CPU scheduler. A "nicer" process (higher number) yields CPU time to other processes. A process with a negative nice value demands more CPU time. Only root can assign negative nice values.

---

## 🟠 Advanced Troubleshooting

**Q9. What is a Zombie process, and how do you kill it?**
> A Zombie (State `Z`) is a process that has finished executing, but its parent has not yet called `wait()` to collect its exit status. It consumes no CPU or memory, only a slot in the process table. 
> You **cannot** kill a zombie directly (even with `kill -9`), because it is already dead. To remove it, you must either fix the parent process or kill the parent process (which causes the zombie to be adopted by `systemd`, which will instantly reap it).

---

**Q10. What is the `D` state in `top`/`ps`, and why is it problematic?**
> The `D` state stands for "Uninterruptible Sleep". The process is waiting directly on hardware (usually disk I/O or network storage like NFS). It is problematic because a process in the `D` state **cannot be killed** (not even by `SIGKILL`) and it continues to add to the system's Load Average. If the hardware is permanently hung, the only resolution is a system reboot.

---

**Q11. You delete a massive log file to free up space, but `df -h` shows the disk is still 100% full. Why?**
> In Linux, if a process still has a file open, deleting the file removes the directory link, but the kernel does not free the disk blocks until the process closes the file descriptor. You must find the process holding the file (using `lsof | grep deleted`) and either restart or kill that process to reclaim the space.

---

**Q12. How does the Linux kernel handle the situation when it completely runs out of physical RAM and Swap?**
> It invokes the OOM (Out Of Memory) Killer. The kernel heuristically determines which process is consuming the most memory (often the one causing the issue) and sends it a `SIGKILL` to forcefully terminate it and free up memory to keep the operating system alive.

---

## 💡 30-Second Interview Answer

> *"In Linux, processes are executing instances of programs, tracked via PIDs in a hierarchical tree rooted at `systemd` (PID 1). We monitor them statically with `ps` or dynamically with `top`/`htop`. Process control relies on Signals—primarily `SIGTERM` (15) for graceful shutdowns and `SIGKILL` (9) for forceful immediate termination. When troubleshooting performance, I look beyond CPU usage to process states, specifically watching for Uninterruptible Sleep ('D' state) which indicates hardware I/O bottlenecks, and Zombies ('Z' state) which indicate poorly written parent processes failing to reap their children."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Troubleshooting Checklists](./13-Troubleshooting-Checklists.md) | [README](./README.md) | [15 - Hands On Terminal Practice](./15-Hands-On-Terminal-Practice.md) |
