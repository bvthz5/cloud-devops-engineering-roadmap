# 18 - Related Topics

Once you have mastered standard process management (signals, states, scheduling), you will encounter advanced mechanisms that control *how* processes interact with the system and each other. These concepts are foundational for modern DevOps, particularly containerization.

---

## 🐳 Namespaces (Process Isolation)

**What it is:** A Linux kernel feature that isolates system resources between processes. A process in one namespace cannot see or interact with resources in another namespace.
**Why it matters:** **Namespaces are the foundation of Docker and containers.** 
When you run a Docker container, it feels like an isolated Virtual Machine because it has its own:
*   **PID Namespace:** The container has its own PID 1, completely unaware of the host's PIDs.
*   **Network Namespace:** It has its own isolated network interfaces (eth0) and IP addresses.
*   **Mount Namespace:** It has its own isolated filesystem hierarchy.
*   *Other namespaces include UTS (hostname), IPC (inter-process communication), and User (UID/GID mapping).*
*   **Command:** `lsns` (List namespaces), `nsenter` (Run program with namespaces of other processes).

---

## 🎛️ Control Groups (cgroups)

**What it is:** A Linux kernel feature that limits, accounts for, and isolates the **resource usage** (CPU, memory, disk I/O, network) of a collection of processes.
**Why it matters:** If namespaces provide the *isolation* for containers, cgroups provide the *resource limits*.
When you run `docker run --memory="256m" --cpus="0.5"`, Docker configures a cgroup in the kernel. If the process inside the container tries to use 300MB of RAM, the kernel's cgroup enforcement will invoke the OOM Killer on that specific process, protecting the host system. `systemd` also uses cgroups heavily to group service processes.
*   **Command:** `systemd-cgls` (View cgroup hierarchy).

---

## 🕵️ System Call Tracing (`strace`)

**What it is:** A powerful diagnostic tool that intercepts and records the system calls (interactions between the process and the kernel) made by a process.
**Why it matters:** When a process hangs or fails with a vague error message (and there are no logs), `strace` reveals exactly what it is trying to do. You can see it attempting to open a file that doesn't exist, failing to bind to a port, or waiting indefinitely on a network socket.
*   **Usage:** `strace -p <PID>` (Attach to a running process), or `strace ./my_program` (Start a program under trace).

---

## 🧵 Threads vs. Processes

**What it is:** While a process is an independent execution unit with its own isolated memory space, a **thread** is a lighter-weight execution unit *within* a process. Multiple threads share the same memory space and file descriptors.
**Why it matters:** Some applications (like Nginx or PostgreSQL) handle concurrency by spawning multiple child *processes*. Others (like Java applications or MySQL) handle concurrency by spawning hundreds of *threads* within a single process.
*   In `ps` or `top`, threads can sometimes appear as separate entities (LWP - Light Weight Processes) depending on your flags (`ps -eLf` or pressing `H` in `top`).

---

## 🛡️ Securing Processes: Capabilities and AppArmor/SELinux

**What it is:** Moving away from the binary "root vs. non-root" permission model.
**Why it matters:** 
*   **Capabilities** break root privileges into smaller pieces. Instead of giving a web server full root access just to bind to port 80, you can give it only the `CAP_NET_BIND_SERVICE` capability.
*   **MAC (Mandatory Access Control)** profiles (like AppArmor or SELinux) enforce rules on what a process can do, *even if the process is running as root*. For example, an AppArmor profile can restrict a Docker container from writing to `/etc`, completely mitigating container escape vulnerabilities.
