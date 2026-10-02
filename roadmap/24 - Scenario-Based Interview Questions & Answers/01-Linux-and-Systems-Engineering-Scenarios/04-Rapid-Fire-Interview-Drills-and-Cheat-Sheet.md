# Linux & Systems Engineering Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Server Clock Drift Breaking Distributed Authentication & Tokens

### 🚨 The Production Scenario
Services across a Kubernetes cluster start failing with `JWT token expired` or `Signature has expired` even for newly issued credentials.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
System clocks on individual nodes drifted by several seconds due to disabled or misconfigured NTP/Chrony daemons. Distributed authentication mechanisms (OAuth2, Kerberos, AWS SigV4, JWT) enforce strict clock skew tolerance windows (typically 5 to 60 seconds).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Inspect system time and synchronization status across nodes using `timedatectl`.
- Step 2: Check Chrony or NTP daemon sync status with `chronyc sources -v`.
- Step 3: Verify time server reachability on UDP port 123.
- Step 4: Force an immediate clock step-synchronization if drift exceeds slew limits.
- Step 5: Configure reliable PTP or Cloud Time Sync (e.g., `169.254.169.123` on AWS/GCP).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Display current system clock, RTC time, and NTP synchronization active status
timedatectl status

# Check clock offset, root delay, and leap status against reference clock
chronyc tracking

# Display status of all upstream NTP server sources
chronyc sources -v

# Force immediate clock synchronization if slew rate is too slow to catch up
chronyc makestep

# Query cloud metadata NTP server for instantaneous offset
ntpdate -q 169.254.169.123

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Clock skew in distributed systems invalidates cryptographic signatures and JWTs. I diagnose this using `timedatectl` and `chronyc tracking` to check clock offset. If drifted, I force an immediate step with `chronyc makestep`, ensure the chrony daemon is active, and configure cloud metadata local time servers (`169.254.169.123` on AWS) which are highly stable and immune to public internet latency."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: High CPU Load Average with Near-Zero CPU Utilization** | `ps -eo state,pid,user,cmd \| awk '$1=="D" {print $0}'` | Load average in Linux counts processes in both `TASK_RUNNING` (State R) and `TAS... |
| **Scenario 2: Disk Full (100% Used) but 'du -sh' Cannot Find the Large Files** | `sudo lsof +L1` | When a file is deleted with `rm` while a running process still holds an open fil... |
| **Scenario 3: Out of Inodes Error While 50GB of Disk Space Remains Free** | `df -i` | Filesystems (such as ext4) reserve a fixed number of inodes during formatting. I... |
| **Scenario 4: Linux OOM Killer Arbitrarily Killing Production PostgreSQL / Java** | `dmesg -T \| grep -i -E 'oom[-_]killer\|killed process'` | The Linux kernel uses the OOM (Out-Of-Memory) killer algorithm based on `oom_sco... |
| **Scenario 5: High Context Switching Degrading System Throughput** | `vmstat 1 5` | Excessive voluntary context switches occur when threads continuously block on mu... |
| **Scenario 6: Zombie Processes Accumulating and Exhausting Kernel PID Space** | `ps -eo stat,ppid,pid,cmd \| grep -E '^Z'` | When a child process terminates, its exit status must be read by its parent proc... |
| **Scenario 7: Network Packet Drops at the OS Kernel Level Under Heavy Load** | `netstat -s \| grep -iE 'listen\|overflowed\|dropped'` | High packet volume can overwhelm the network interface card (NIC) ring buffers a... |
| **Scenario 8: SSH Access Fails with 'Connection Closed by Remote Host' during Production Outage** | `ssh -vvv -o ConnectTimeout=10 user@server.domain.com` | Common root causes include: 1) SSH daemon process limits reached (`MaxStartups` ... |
| **Scenario 9: Read-Only Filesystem Remount on Production Storage Array** | `dmesg -T \| grep -iE 'ext4\|xfs\|io error\|remount'` | Filesystem drivers (like ext4) are configured by default in `/etc/fstab` with `e... |
| **Scenario 10: Server Clock Drift Breaking Distributed Authentication & Tokens** | `timedatectl status` | System clocks on individual nodes drifted by several seconds due to disabled or ... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Linux & Systems Engineering Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Computer Networking & DNS Interview Scenarios: Production Incidents & Triage Scenarios →](../02-Computer-Networking-and-DNS-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

