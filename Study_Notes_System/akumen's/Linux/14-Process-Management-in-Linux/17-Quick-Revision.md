# 17 - Quick Revision Cheat Sheet

High-density 5-minute summary for fast review before interviews, exams, or on-call incident response.

---

## 🚦 Process States

| State | Name | Meaning | Can you kill it? |
| :---: | :--- | :--- | :---: |
| **R** | Running | Currently on CPU or in run queue. | Yes |
| **S** | Sleeping | Waiting for an event/input (Interruptible). | Yes |
| **D** | Uninterruptible | Waiting for hardware I/O (Disk/Network). | **NO** |
| **T** | Stopped | Suspended via `Ctrl+Z` (SIGSTOP). | Yes |
| **Z** | Zombie | Dead, but parent hasn't reaped it yet. | **NO** (Kill parent) |

---

## 📡 The Critical Signals

| Signal | Number | Description | Use Case |
| :--- | :---: | :--- | :--- |
| **SIGTERM** | 15 | Polite request to terminate. | `kill PID` (Default). Always try this first. |
| **SIGKILL** | 9 | Forceful, immediate destruction. | `kill -9 PID`. Last resort for frozen processes. |
| **SIGHUP** | 1 | Terminal hangup. | Used by daemons to reload configs without downtime. |
| **SIGINT** | 2 | Keyboard interrupt. | Sent by `Ctrl+C`. |
| **SIGSTOP** | 19 | Pause execution. | Sent by `Ctrl+Z`. Resumed with `SIGCONT`. |

---

## 🧰 The Core Toolset

| Tool | Purpose | Key Flags/Usage |
| :--- | :--- | :--- |
| `ps` | Static Snapshot | `ps aux` (all processes), `ps -ef` (shows PPID) |
| `top`/`htop`| Dynamic Monitoring | Interactive. `htop` is visually superior. |
| `pgrep` | Find PID by Name | `pgrep nginx` |
| `pkill` | Kill by Name | `pkill -9 -u alice` (Kills all Alice's processes) |
| `lsof` | List Open Files | `lsof -p PID`, `lsof -i :80` (Find port conflicts) |
| `free -h` | Memory Usage | Look at `available` RAM, not `free` RAM. |
| `uptime` | Load Average | 1, 5, 15 min averages. Compare against CPU cores. |

---

## 🎛️ Job Control & Persistence

*   **`&`**: Put a job in the background (`sleep 100 &`).
*   **`Ctrl+Z`**: Suspend a foreground job.
*   **`bg` / `fg`**: Move suspended jobs to background/foreground.
*   **`nohup`**: Ignore `SIGHUP` (`nohup ./script.sh &`). Survives disconnect.
*   **`tmux`**: The professional way to run persistent, attachable terminal sessions.

---

## ⚖️ Niceness (Priority)

*   **Scale:** `-20` (Highest Priority, Needs Root) to `+19` (Lowest Priority, Very Nice).
*   **Default:** `0`.
*   **`nice`**: Start a process with priority (`nice -n 15 ./script.sh`).
*   **`renice`**: Change a running process (`renice -n 10 -p PID`).

---

## ⚙️ systemd & /proc

*   **systemd:** PID 1. Manages daemons.
    *   `systemctl start/stop/restart/reload/status <service>`
    *   `journalctl -u <service> -f` (View logs).
*   **`/proc`:** Virtual filesystem exposing kernel memory.
    *   `/proc/PID/cmdline`: Exact command used to run it.
    *   `/proc/PID/fd/`: Open file descriptors (can be used to recover deleted files!).

---

## 🎯 30-Second Interview Answer

> *"Linux processes are executing instances of programs, organized in a tree rooted at PID 1 (`systemd`). We monitor them dynamically with `htop` and statically with `ps`. Processes transition through states like Running, Sleeping, Uninterruptible Sleep (D-state, indicating I/O blocks), and Zombie. We control them using Signals—primarily `SIGTERM` (15) for graceful exits and `SIGKILL` (9) as a last resort. For long-running administrative tasks, we use terminal multiplexers like `tmux` to protect against `SIGHUP` disconnects, and adjust CPU scheduling priority using `nice`."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - MCQs](./16-MCQs.md) | [README](./README.md) | [18 - Related Topics](./18-Related-Topics.md) |
