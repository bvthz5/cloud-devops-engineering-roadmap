# 07 - Process Priority: nice and renice

In a busy Linux system, multiple processes compete for CPU time. The kernel's CPU scheduler decides which process gets to use the CPU next. 

You can influence this decision by adjusting a process's **"niceness"**.

---

## 😇 What is "Niceness"?

Think of niceness as how polite a process is to other processes waiting in line for the CPU.

*   A **highly nice** process says, "Take your time, I can wait." (Low priority).
*   A **not nice** process says, "Get out of my way, I need the CPU now!" (High priority).

### The Niceness Scale
The scale ranges from **-20 to +19**.
*   **-20:** Least nice. Highest possible CPU priority.
*   **0:** The default niceness for all new processes.
*   **+19:** Most nice. Lowest possible CPU priority.

### The Golden Rule of nice
**Any user** can make their own processes *nicer* (increase the number from 0 to +19) to yield CPU to the rest of the system.
**Only the root user** can make processes *less nice* (decrease the number below 0) to demand more CPU.

---

## ⚖️ Starting a Process with Priority (`nice`)

If you are about to run a massive data compression script that will take hours and you don't want it to slow down the web server running on the same machine, start it with a high nice value.

```bash
# Start a script very nicely (low priority)
nice -n 15 ./heavy_backup.sh
```

If you are root and need to run an emergency diagnostic script that must not be interrupted, start it with a negative nice value.

```bash
# Start a script with high priority (requires sudo)
sudo nice -n -10 ./emergency_diag.sh
```

*(Note: If you just use `nice command` without `-n`, it defaults to adding +10 to the niceness).*

---

## 🎛️ Changing Priority on the Fly (`renice`)

If a process is already running and you notice it is consuming 99% of the CPU, you can throttle it without killing it using `renice`.

First, find the PID using `top` or `ps`. Let's say the PID is 8842.

```bash
# Make the running process much nicer (lower priority)
renice -n 10 -p 8842
```

If a critical database query is starving for CPU, you can give it a boost (requires root):

```bash
sudo renice -n -5 -p 8842
```

---

## 👁️ Viewing Niceness

When you run `top`, the `NI` column shows the niceness value. The `PR` (Priority) column shows the actual priority mapped by the kernel (which incorporates the nice value).

When you run `ps aux`, the `STAT` column indicates priority changes:
*   `N`: Low priority (Nice value > 0).
*   `<`: High priority (Nice value < 0).

Example output in `ps aux`:
```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
alice     8842 99.0  0.1  12345  4567 pts/0    RN   10:00   1:23 ./heavy_backup.sh
```
*(The `RN` means it is Running, and it is low priority/Nice).*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Detaching Processes nohup tmux](./06-Detaching-Processes-nohup-tmux.md) | [README](./README.md) | [08 - Resource Monitoring CPU Memory IO](./08-Resource-Monitoring-CPU-Memory-IO.md) |
