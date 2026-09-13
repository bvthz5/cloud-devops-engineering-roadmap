# 03 - Viewing Processes: ps, top, and htop

To manage processes, you first need to see them. Linux provides two primary approaches to viewing processes: **static snapshots** (`ps`) and **dynamic interactive monitoring** (`top`, `htop`).

---

## 📸 1. Taking Snapshots with `ps`

The `ps` (process status) command captures a snapshot of currently running processes. Unlike `top`, it does not update continuously.

### Basic Usage
Running `ps` without arguments only shows processes running in your current terminal session.
```bash
ps
# Output: PID, TTY (terminal), TIME (CPU time used), CMD
```

### The Standard Standard: `ps aux`
This is the most famous combination of flags (BSD syntax). It lists **every** process on the system, regardless of who owns it or if it has a terminal attached.

*   **`a`**: Show processes for all users.
*   **`u`**: Display in a user-oriented format (shows User, CPU%, MEM%, Start Time).
*   **`x`**: Show processes not attached to a terminal (background daemons).

```bash
ps aux
```

**Understanding `ps aux` Columns:**
*   **USER:** The user account running the process.
*   **PID:** Process ID.
*   **%CPU / %MEM:** Percentage of CPU/RAM currently used.
*   **VSZ:** Virtual memory size (how much memory the process *thinks* it has).
*   **RSS:** Resident Set Size (how much *actual* physical RAM the process is using).
*   **STAT:** Process state (R, S, D, Z, etc.).
*   **START:** When the process started.
*   **COMMAND:** The command that initiated the process.

### The SysV Standard: `ps -ef`
This is the standard UNIX (System V) equivalent to `ps aux`. It shows slightly different columns.

*   **`-e`**: Every process.
*   **`-f`**: Full format listing.

```bash
ps -ef
```
*Crucially, `ps -ef` clearly shows the **PPID** (Parent PID), which `ps aux` hides by default.*

### Filtering `ps` Output
Because `ps aux` outputs hundreds of lines, it is almost always piped to `grep`.

```bash
# Find all nginx processes
ps aux | grep nginx

# Find processes owned by 'alice'
ps -u alice
```

---

## 🔍 2. Finding PIDs with `pgrep` and `pidof`

When you only need the PID of a process (usually so you can kill it), parsing `ps` is overkill.

**`pgrep` (Pattern Grep):** Searches for processes by name/pattern and returns only the PID.
```bash
pgrep sshd
# Output: 852 1405 1422
```

**`pidof`:** Returns the PID of an exact program name.
```bash
pidof nginx
# Output: 994 995
```

---

## 📈 3. Dynamic Monitoring with `top`

`top` provides a real-time, continually updating view of system processes. It is installed on every Linux system.

```bash
top
```

**The `top` Interface:**
1.  **System Summary (Top section):**
    *   **Uptime & Load Average:** Critical for assessing system stress (1min, 5min, 15min averages).
    *   **Tasks:** Total processes, running, sleeping, stopped, zombie.
    *   **%Cpu(s):** Breakdown of CPU usage (`us` = user, `sy` = system/kernel, `id` = idle, `wa` = waiting for I/O).
    *   **KiB Mem / Swap:** Free, used, and cached memory.
2.  **Process List (Bottom section):** Updates every 3 seconds by default, sorted by CPU usage.

**Interactive Commands while running `top`:**
*   **`q`**: Quit.
*   **`M`**: Sort by Memory usage (Shift+M).
*   **`P`**: Sort by CPU usage (Shift+P, default).
*   **`k`**: Kill a process (prompts for PID).
*   **`c`**: Toggle showing the full command line vs. just the program name.

---

## 🌈 4. Modern Monitoring with `htop`

`htop` is a third-party enhancement to `top`. While it must usually be installed manually (`apt install htop` or `yum install htop`), it is universally preferred by DevOps engineers for interactive troubleshooting.

```bash
htop
```

**Why `htop` is better than `top`:**
*   **Visual Bars:** CPU, Memory, and Swap are displayed with color-coded bars, making bottlenecks instantly visible.
*   **Vertical & Horizontal Scrolling:** You can scroll through the entire process list and see long command lines.
*   **Mouse Support:** You can click columns to sort them.
*   **Tree View:** Press `F5` to view processes in their parent/child tree hierarchy.
*   **Easier Actions:** Select a process and press `F9` to send a kill signal, without needing to type the PID.
