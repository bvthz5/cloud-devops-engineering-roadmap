# Linux & Systems Engineering Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Linux OOM Killer Arbitrarily Killing Production PostgreSQL / Java

### 🚨 The Production Scenario
During memory pressure, the Linux kernel terminates the primary PostgreSQL master instance instead of non-critical background jobs, causing database downtime.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Linux kernel uses the OOM (Out-Of-Memory) killer algorithm based on `oom_score`, which is calculated from the proportion of RAM consumed by the process multiplied by its `oom_score_adj`. Because PostgreSQL or a JVM heap consumes the largest memory footprint, the kernel algorithm naively flags it as the top candidate to reclaim memory.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Examine kernel OOM termination events in `dmesg -T` and `/var/log/messages`.
- Step 2: Adjust `oom_score_adj` for critical stateful daemons to `-1000` (immune) or `-900`.
- Step 3: Configure Linux kernel virtual memory overcommit settings (`sysctl vm.overcommit_memory=2`).
- Step 4: Isolate memory limits using systemd slices or cgroups (`MemoryMax`, `MemoryHigh`).
- Step 5: Implement proactive alerting at 85% memory consumption before kernel OOM activates.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Extract kernel OOM kill events with exact timestamps, RSS pages, and victims
dmesg -T | grep -i -E 'oom[-_]killer|killed process'

# Protect critical database process from OOM Killer by setting score adjustment to -1000
echo -1000 | sudo tee /proc/<PG_PID>/oom_score_adj

# Inspect current calculated OOM score (higher values are killed first)
cat /proc/<PID>/oom_score

# Strict memory overcommit: refuse allocations exceeding physical RAM + swap ratio
sysctl -w vm.overcommit_memory=2; sysctl -w vm.overcommit_ratio=80

# Persist OOM protection across service restarts via systemd unit property
systemctl set-property postgresql.service OOMScoreAdjust=-900

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The Linux OOM killer targets processes with the highest `oom_score`, which naturally penalizes databases and heavy application runtimes. To protect mission-critical workloads, I set `OOMScoreAdjust=-900` via systemd, tune kernel overcommit parameters (`vm.overcommit_memory=2`), and enforce cgroup memory boundaries on rogue batch scripts to guarantee that non-critical workers are terminated instead of core stateful services."

---

## 📌 Scenario 5: High Context Switching Degrading System Throughput

### 🚨 The Production Scenario
A 16-core API gateway shows 90% CPU utilization, but profiling reveals over 350,000 context switches per second, with CPUs spending 40% of time in kernel mode (`%sys`).

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Excessive voluntary context switches occur when threads continuously block on mutexes, semaphores, or synchronous I/O. Involuntary context switches occur when thread count drastically exceeds CPU cores, causing the Linux Completely Fair Scheduler (CFS) to repeatedly preempt threads, invalidating CPU L1/L2 caches and translation lookaside buffers (TLBs).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Quantify voluntary vs involuntary context switches using `pidstat -w 1`.
- Step 2: Identify whether the issue is system-wide or isolated to a specific multi-threaded runtime.
- Step 3: Tune worker thread pool sizes to match available logical CPU cores.
- Step 4: Pin latency-sensitive threads to dedicated CPU cores using `taskset` or `cpuset`.
- Step 5: Transition synchronous blocking I/O calls to non-blocking epoll event loops.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Monitor system-wide context switches (cs column) and interrupts (in column) per second
vmstat 1 5

# Break down voluntary (cswch/s) and involuntary (nvcswch/s) context switches per process
pidstat -w 1 5

# Profile real-time kernel vs user space function call overheads
perf top

# Pin process and its worker threads to CPU cores 0 through 3 (CPU affinity)
taskset -cp 0-3 <PID>

# Summarize system call distribution and identify syscalls causing frequent mode switches
strace -c -p <PID>

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "High context switching destroys CPU throughput by repeatedly evicting cache lines and flushing TLBs. I differentiate between voluntary switches (threads blocking on locks or I/O) and involuntary switches (CPU preemption from over-threading) using `pidstat -w`. If involuntary is high, I down-tune thread pool size to match physical core counts. If voluntary is high, I trace lock contention using `perf` and migrate blocking I/O to async event loops."

---

## 📌 Scenario 6: Zombie Processes Accumulating and Exhausting Kernel PID Space

### 🚨 The Production Scenario
A backend batch server fails to spawn new subshells or background tasks with `fork: retry: Resource temporarily unavailable`. `ps` reveals thousands of `<defunct>` processes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When a child process terminates, its exit status must be read by its parent process using the `wait()` or `waitpid()` system call. If the parent fails to invoke `wait()` or ignores `SIGCHLD`, the child remains in the process table as a Zombie (State Z) to preserve its exit code, eventually consuming all allocated PIDs (`/proc/sys/kernel/pid_max`).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check available PID limits via `cat /proc/sys/kernel/pid_max` and current allocated count.
- Step 2: List all zombie processes and identify their parent process ID (PPID).
- Step 3: Note: You cannot `kill -9` a zombie because it is already dead. You must signal or terminate the parent process.
- Step 4: Send `SIGCHLD` to the parent process to prompt it to reap its dead children.
- Step 5: If the parent daemon is unresponsive, restart the parent process so orphaned zombies are reparented to PID 1 (init/systemd), which immediately reaps them.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Locate all zombie processes (State Z) and display their parent PID (PPID)
ps -eo stat,ppid,pid,cmd | grep -E '^Z'

# Signal the parent process to execute waitpid() and collect terminated child exit codes
kill -s SIGCHLD <PARENT_PID>

# Display the maximum system PID limit
cat /proc/sys/kernel/pid_max

# Restart the parent service to eliminate accumulated zombies
systemctl restart <parent_service>

# Ensure containers run with Tini/dumb-init as PID 1 to reap zombie processes
docker run --init -d myapp:latest

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A zombie process has already terminated its execution and holds zero memory or CPU, but retains an entry in the kernel process table until its parent calls `wait()`. You cannot kill a zombie with `kill -9`. I identify the broken parent PID using `ps -eo stat,ppid,pid,cmd | grep ^Z`, send `SIGCHLD` to the parent to prompt reaping, and if necessary restart the parent. In containers, I always ensure an init process like Tini runs as PID 1 to prevent zombie leaks."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Linux & Systems Engineering Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Linux & Systems Engineering Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

