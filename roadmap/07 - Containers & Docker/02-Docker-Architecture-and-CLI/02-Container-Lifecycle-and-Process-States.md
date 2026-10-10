# 02 - Container Lifecycle and Process States

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to the container finite state machine, process lifecycle states, PID 1 signal propagation, `SIGTERM` vs. `SIGKILL`, exit codes forensics, and the zombie reaping problem.

---

## 🏛️ 1. What Is a Container Lifecycle & What Is a Container Fundamentally?

To understand how containers start, run, pause, and terminate, one must first dismantle a common misconception: **a container is not a lightweight virtual machine**. There is no guest operating system kernel booting inside a container, and there is no hypervisor running in the background.

Fundamentally, **a container is an ordinary Linux process running directly on the host kernel**, constrained by Linux **Namespaces** (for visibility isolation), **Control Groups** (for resource limits), and an **OverlayFS union filesystem** (for an isolated root directory).

Because a container is defined entirely by the primary process executing inside it (designated as **PID 1** inside the container's PID namespace), **the lifecycle of the container is strictly bound to the lifecycle of PID 1**. The millisecond PID 1 completes its execution, crashes, or is terminated by a signal, the container instantly transitions from the `running` state to the `exited` state.

---

## ⚙️ 2. The Complete Container Finite State Machine

Docker tracks every container through a deterministic finite state machine consisting of seven distinct operational states:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   CONTAINER FINITE STATE MACHINE                       │
└────────────────────────────────────────────────────────────────────────┘

                 docker create
   [ (Absent) ] ──────────────► [ CREATED ]
                                    │
                                    │ docker start / docker run
                                    ▼
           docker pause         [ RUNNING ] ◄──────────────┐
        ┌───────────────────────┤         ├──────────────┐ │
        │                       └────┬────┘              │ │ docker unpause
        ▼                            │                   │ │
   [ PAUSED ] ───────────────────────┘                   │ │
                     docker unpause                      ▼ │
                                                 [ RESTARTING ]
                     docker stop (SIGTERM ──► SIGKILL)   │
                                     │                   │ (Max retries exceeded)
                                     ▼                   │
                                 [ EXITED ] ◄────────────┘
                                     │
                                     │ docker rm
                                     ▼
                                [ (Absent) ]
```

### Detailed Breakdown of the Seven States:
1. **`created`:** Docker has parsed the configuration, allocated the writable layer on disk, and reserved the network endpoint, but the primary application process has not yet been spawned.
2. **`running`:** The primary process (PID 1) is actively executing on the host CPU cores, actively listening on network sockets, and servicing requests.
3. **`paused`:** All processes within the container are frozen in place using the Linux kernel's **cgroup freezer subsystem**. The processes consume **zero CPU cycles**, but their memory state remains fully preserved in physical RAM. Resuming a paused container with `docker unpause` is instantaneous.
4. **`restarting`:** The primary process has exited, and the Docker daemon is currently evaluating the container's configured restart policy (`on-failure`, `always`, `unless-stopped`) to instantiate a fresh process.
5. **`removing`:** The Docker daemon is actively tearing down virtual network interfaces, unmounting filesystem layers, and deleting metadata.
6. **`exited`:** The primary process has terminated (either normally with exit code `0` or abnormally with an error exit code). The container's root filesystem and logs remain intact on host disk, allowing administrators to inspect output via `docker logs` and examine state.
7. **`dead`:** An uncommon error state where the Docker daemon attempted to remove or stop the container but encountered an operating system lock or unmount error.

---

## 🔬 3. Process Termination: `SIGTERM` vs. `SIGKILL`

When an administrator or automated deployment script commands Docker to stop a container via `docker stop <container-id>`, Docker executes a two-phase graceful termination sequence:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   GRACEFUL TERMINATION TIMELINE                        │
└────────────────────────────────────────────────────────────────────────┘

 docker stop executed
   │
   ▼
 Phase 1: Graceful Termination Request
 Docker transmits SIGTERM (Signal 15) to PID 1
   │
   ├─► Application handles signal:
   │   - Stops accepting new HTTP connections
   │   - Completes in-flight database transactions
   │   - Flushes write buffers to disk
   │   - Closes open network sockets
   │   - Exits cleanly with Exit Code 0 or 143! (Process ends)
   │
   ▼ (Did application exit within Grace Period? Default: 10 seconds)
   │
   ├─► NO (Application is hung or ignoring SIGTERM)
   │
   ▼
 Phase 2: Brutal Kernel Termination
 Docker transmits SIGKILL (Signal 9) to PID 1
   │
   ▼
 Linux kernel abruptly purges process memory immediately! (Exit Code 137)
```

### Key Operational Controls:
- **`docker stop -t <seconds>`:** Extends the grace period (e.g., `docker stop -t 60 postgres_db` gives a database up to 60 seconds to checkpoint dirty memory pages to disk before being killed).
- **`docker kill`:** Skips the graceful `SIGTERM` phase entirely, immediately transmitting `SIGKILL` (or an arbitrary signal like `docker kill -s SIGHUP <id>`).
- **`STOPSIGNAL` Instruction:** By default, Docker sends `SIGTERM`. Some applications require different signals for graceful shutdown (for example, Nginx requires `SIGQUIT` for graceful worker shutdown). You can declare this directly in the Dockerfile:
  ```dockerfile
  STOPSIGNAL SIGQUIT
  ```

---

## 📊 4. Container Exit Codes Forensics Matrix

When a container exits unexpectedly, the Docker CLI reports an **Exit Code** that reveals precisely why the application failed:

| Exit Code | Standard Meaning | Root Cause & DevOps Diagnostic Analysis |
| :---: | :--- | :--- |
| **`0`** | Success / Clean Exit | Application completed its task successfully (e.g. database migration finished). |
| **`1`** | General Application Error | Uncaught application exception, missing configuration key, or invalid syntax. |
| **`126`** | Command Invoked Cannot Execute | The target file exists, but lacks executable permissions (`chmod +x missing`). |
| **`127`** | File or Command Not Found | Typo in entrypoint binary path, or missing interpreter (e.g. `/bin/bash` missing in Alpine). |
| **`137`** | Fatal Signal 9 (`SIGKILL` / OOM) | Process terminated brutally: either by `docker kill`, timeout expiry, or the **Linux kernel Out-Of-Memory (OOM) Killer**! (128 + 9 = 137). |
| **`139`** | Segmentation Fault (`SIGSEGV`) | Native memory corruption in compiled code (C/C++/Go/Rust) (128 + 11 = 139). |
| **`143`** | Graceful Termination (`SIGTERM`) | Container received a graceful stop request from Docker and exited cleanly (128 + 15 = 143). |

---

## 🧟 5. The PID 1 Zombie Reaping Problem in Containers

In standard Unix operating systems, the `init` or `systemd` process runs as **PID 1**. PID 1 has two critical system responsibilities:
1. **Signal Forwarding:** Receiving OS signals and forwarding them down to child worker processes.
2. **Zombie Reaping:** When a child process terminates, it enters a "zombie" state until its parent process reads its exit status via the `wait()` system call. If the parent terminates before the child, the orphaned zombie process is adopted by PID 1, which reaps it from the process table.

### Why Applications Make Poor PID 1 Processes:
Most application runtimes (such as Node.js, Python, or Java) were never engineered to act as system init systems. When executed as PID 1:
- They often ignore `SIGTERM` signals unless custom signal listeners are explicitly coded.
- They fail to reap orphaned child processes. If your application spawns background shell commands (`fork()`), dead processes accumulate indefinitely in the process table as **zombies (`<defunct>`)**, eventually exhausting host PID limits.

### The Solution: Using an Init Process (`tini` or `dumb-init`)
Modern Docker provides native support for the ultra-lightweight **`tini`** init process via the `--init` CLI flag:
```bash
# Docker automatically inserts tini as PID 1 to reap zombies and forward signals!
docker run -d --init --name my_app node:20-alpine node server.js
```
In Docker Compose:
```yaml
services:
  web:
    image: my_app
    init: true    # Activates tini as container PID 1!
```

---

## 📋 6. Summary and Navigation

Understanding the container finite state machine, graceful termination workflows, exit code forensics, and PID 1 signal forwarding transforms opaque container failures into transparent, debuggable events. By configuring appropriate stop timeouts and activating init subreapers, DevOps engineers ensure predictable, resilient microservice operations.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Docker Engine Architecture and Subsystems](./01-Docker-Engine-Architecture-and-Subsystems.md) | [Index](../../../README.md) | [03 - Essential Docker CLI Commands →](./03-Essential-Docker-CLI-Commands.md) |
