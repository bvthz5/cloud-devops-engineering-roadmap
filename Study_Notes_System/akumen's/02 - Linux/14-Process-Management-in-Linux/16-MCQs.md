# 16 - MCQs

15 multiple-choice questions to test your understanding. Attempt each before revealing the answer.

---

**Q1. What is PID 1 on a modern Linux system?**

- A. The kernel
- B. bash
- C. systemd
- D. sshd

<details>
<summary>Answer</summary>

**C — systemd**

`systemd` (or historically `init`) is the first process started by the kernel and is the ancestor of all other processes.

</details>

---

**Q2. In `ps aux` output, what does the `S` under the STAT column mean?**

- A. Stopped
- B. Sleeping (Interruptible)
- C. Sleeping (Uninterruptible)
- D. System Process

<details>
<summary>Answer</summary>

**B — Sleeping (Interruptible)**

Most processes spend their time in this state, waiting for user input, network data, or a timer to wake them up.

</details>

---

**Q3. Which signal is sent by default when you run the `kill <PID>` command?**

- A. `SIGINT` (2)
- B. `SIGKILL` (9)
- C. `SIGTERM` (15)
- D. `SIGHUP` (1)

<details>
<summary>Answer</summary>

**C — `SIGTERM` (15)**

It politely asks the process to terminate, allowing it to clean up before exiting.

</details>

---

**Q4. You want a process to continue running even after you close your SSH terminal session. Which command should you use?**

- A. `nohup`
- B. `bg`
- C. `nice`
- D. `&`

<details>
<summary>Answer</summary>

**A — `nohup`**

`nohup` ignores the `SIGHUP` signal sent by the shell when the terminal is closed. `&` only puts it in the background, it does not protect it from disconnects.

</details>

---

**Q5. A process has a state of `Z`. How should you resolve this?**

- A. Run `kill -9 <PID>` on the zombie process.
- B. Run `renice` to give it more CPU time to finish.
- C. Restart the server immediately; the kernel is panicking.
- D. Find the parent process and kill it (or fix its code).

<details>
<summary>Answer</summary>

**D — Find the parent process and kill it (or fix its code).**

You cannot kill a Zombie because it is already dead. You must force the parent to reap it, or kill the parent so `systemd` adopts and reaps the zombie.

</details>

---

**Q6. What keyboard shortcut suspends a foreground job and returns you to the shell prompt?**

- A. `Ctrl+C`
- B. `Ctrl+Z`
- C. `Ctrl+D`
- D. `Ctrl+L`

<details>
<summary>Answer</summary>

**B — `Ctrl+Z`**

This sends `SIGSTOP` to the foreground process.

</details>

---

**Q7. Which command lists all open files and network sockets associated with a running process?**

- A. `ps -ef`
- B. `lsof`
- C. `iostat`
- D. `free`

<details>
<summary>Answer</summary>

**B — `lsof`**

"List Open Files" is essential for finding which process is holding onto a port or a deleted log file.

</details>

---

**Q8. If you want to increase the priority of a process (give it more CPU time), which nice value should you use?**

- A. `19`
- B. `0`
- C. `10`
- D. `-10`

<details>
<summary>Answer</summary>

**D — `-10`**

Negative nice values indicate "less nice" (higher priority). Only root can assign negative nice values.

</details>

---

**Q9. In the `/proc` filesystem, what does `/proc/<PID>/cmdline` contain?**

- A. The environment variables of the process.
- B. A log of commands the process has executed.
- C. The exact command and arguments used to start the process.
- D. The CPU usage history of the process.

<details>
<summary>Answer</summary>

**C — The exact command and arguments used to start the process.**

It is very useful for seeing exactly how a confusing process was launched.

</details>

---

**Q10. What does the command `pkill -9 nginx` do?**

- A. Gracefully stops all processes named nginx.
- B. Forcefully kills all processes containing the string "nginx" in their name.
- C. Kills the process with PID 9.
- D. Suspends the nginx process.

<details>
<summary>Answer</summary>

**B — Forcefully kills all processes containing the string "nginx" in their name.**

`pkill` finds processes by pattern and sends the specified signal (in this case, 9 / `SIGKILL`).

</details>

---

**Q11. You run `uptime` and see a load average of `15.00`. Your system has 2 CPU cores. What does this mean?**

- A. The system is operating normally at 15% capacity.
- B. The system is severely overloaded; processes are waiting in line for CPU/IO.
- C. The system has been up for 15 hours.
- D. 15 processes are currently in the Zombie state.

<details>
<summary>Answer</summary>

**B — The system is severely overloaded; processes are waiting in line for CPU/IO.**

A load average higher than the number of CPU cores indicates congestion. 15.00 on 2 cores is massive overload.

</details>

---

**Q12. Which command is used to bring a background job back to the interactive terminal?**

- A. `bg`
- B. `resume`
- C. `fg`
- D. `jobs`

<details>
<summary>Answer</summary>

**C — `fg`**

`fg` (foreground) brings the job back to the terminal.

</details>

---

**Q13. You need to run a script at exactly 2:00 AM every Tuesday. Which tool is best suited for this?**

- A. `systemd`
- B. `at`
- C. `tmux`
- D. `cron`

<details>
<summary>Answer</summary>

**D — `cron`**

`cron` is designed specifically for recurring, scheduled tasks. `at` is for one-time tasks.

</details>

---

**Q14. What does the `D` state signify in `top`?**

- A. The process is Daemonized.
- B. The process is Dead (Zombie).
- C. The process is in Uninterruptible Sleep (waiting for I/O).
- D. The process is Detached.

<details>
<summary>Answer</summary>

**C — The process is in Uninterruptible Sleep (waiting for I/O).**

This usually indicates a disk or network storage bottleneck. The process cannot be killed in this state.

</details>

---

**Q15. Why is `tmux` preferred over `nohup` for long-running administrative tasks?**

- A. `tmux` uses less CPU than `nohup`.
- B. `tmux` automatically restarts failed scripts.
- C. `tmux` allows you to detach from a session and re-attach later to interact with it.
- D. `tmux` forces the process to run with a negative nice value.

<details>
<summary>Answer</summary>

**C — `tmux` allows you to detach from a session and re-attach later to interact with it.**

`nohup` only redirects output to a file; you cannot interact with the process once you leave the terminal.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Hands On Terminal Practice](./15-Hands-On-Terminal-Practice.md) | [README](./README.md) | [17 - Quick Revision](./17-Quick-Revision.md) |
