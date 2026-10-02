# Linux & Systems Engineering Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: High CPU Load Average with Near-Zero CPU Utilization

### 🚨 The Production Scenario
A production web node reports a load average of 48.0 on an 8-core CPU, yet CPU utilization in `top` shows 96% idle. User requests are timing out.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Load average in Linux counts processes in both `TASK_RUNNING` (State R) and `TASK_UNINTERRUPTIBLE` (State D). State D processes are blocked waiting on uninterruptible kernel resources, typically disk I/O, network storage (NFS), or hardware locks. Here, an NFS mount hung due to a severed network socket, trapping worker threads in kernel wait queues.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check process states using `ps -eo state,pid,cmd | grep ^D` to identify trapped threads.
- Step 2: Inspect disk and storage queue metrics using `iostat -xz 1 5` to locate saturated block devices.
- Step 3: Check kernel dmesg and system logs via `dmesg -T | grep -iE 'nfs|blocked|hung'` for kernel stall warnings.
- Step 4: Remount or force-unmount the unresponsive NFS share using `umount -f -l /mnt/nfs_share`.
- Step 5: Configure NFS mount options with `soft,timeo=50,retrans=2` to prevent infinite kernel deadlocks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify all processes currently blocked in Uninterruptible Sleep (State D)
ps -eo state,pid,user,cmd | awk '$1=="D" {print $0}'

# Print the exact kernel function where the blocked process is sleeping (e.g. nfs_wait_bit_killable)
cat /proc/<PID>/wchan

# Display the kernel call stack of the hanging thread to trace the blocking lock
cat /proc/<PID>/stack

# Verify device queue depth (%util, await, r_await, w_await)
iostat -xz 1 5

# Lazy force unmount the severed network share to release stuck I/O queues
sudo umount -f -l /mnt/stale_mount

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When CPU load is high while CPU utilization is near zero, it signifies processes blocked in Uninterruptible Sleep (State D), usually caused by I/O starvation or hung network filesystems. I triage this by inspecting `/proc/[PID]/wchan` and `/proc/[PID]/stack` to identify the exact kernel syscall, checking `iostat` for disk saturation, and force-unmounting any disconnected storage mounts with lazy unmount (`umount -l`). To permanently mitigate, I ensure all remote mounts use soft timeouts rather than hard blocking."

---

## 📌 Scenario 2: Disk Full (100% Used) but 'du -sh' Cannot Find the Large Files

### 🚨 The Production Scenario
A production server alert fires for `/var/log` at 100% capacity (`df -h`). However, running `du -sh /var/log/*` accounts for only 4GB out of 100GB. New writes fail with 'No space left on device'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When a file is deleted with `rm` while a running process still holds an open file descriptor to it, the directory entry (dentry) is unlinked, but the inode and data blocks cannot be released by the kernel until the process terminates or closes the descriptor. Disk space remains consumed on the filesystem but invisible to directory crawlers like `du`.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Identify deleted files still held open by running processes using `lsof +L1`.
- Step 2: Locate the owning process PID and process name holding the deleted file descriptor.
- Step 3: Safely truncate the file via the `/proc/<PID>/fd/<FD>` virtual filesystem without restarting the service.
- Step 4: Signal the daemon (e.g., `systemctl reload logrotate` or `pkill -HUP`) to re-open its log files.
- Step 5: Configure logrotate with `copytruncate` or proper postrotate reload scripts to prevent future descriptor leaks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List all unlinked files with a reference count greater than 0 that are consuming disk space
sudo lsof +L1

# Filter processes holding open handles to deleted log files
sudo lsof | grep '(deleted)'

# Truncate the open file descriptor to 0 bytes directly through procfs without stopping the application
: > /proc/<PID>/fd/<FD_NUM>

# Instruct the daemon to close existing file handles and open fresh log files cleanly
systemctl reload nginx

# Sanity check for other large files hidden under mount points
find / -xdev -size +100M -exec ls -lh {} \;

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "This occurs because Linux unlinks the directory reference upon `rm`, but the inode blocks remain allocated as long as an active process keeps the file descriptor open. `du` traverses the directory tree and cannot see unlinked files, while `df` queries the filesystem superblock directly. I solve this immediately in production by running `lsof +L1` to find the offending PID, and zeroing the file via `: > /proc/<PID>/fd/<FD>` to reclaim disk space instantly without killing the production process."

---

## 📌 Scenario 3: Out of Inodes Error While 50GB of Disk Space Remains Free

### 🚨 The Production Scenario
Application logs report `java.io.IOException: No space left on device`. Running `df -h` shows 50GB available (40% free), but no new files can be created.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Filesystems (such as ext4) reserve a fixed number of inodes during formatting. If an application (e.g., mail server spool, cache directory, or sessions store) generates millions of tiny micro-files (0-1KB), the inode table exhausts completely before raw block storage is consumed.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Verify inode exhaustion using `df -i`.
- Step 2: Locate directories containing excessive file counts by crawling the filesystem with `find`.
- Step 3: Safely purge accumulated micro-files using `rsync` or streaming `find -delete` to avoid argument list overflow (`E2BIG`).
- Step 4: Implement automated cron purges and directory cleanup routines.
- Step 5: For persistent micro-file workloads, evaluate filesystems like XFS (dynamic inode allocation) or consolidate small files.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Display inode usage, free inodes, and percentage utilization across all mounted filesystems
df -i

# Find the top 20 directories containing the highest number of individual files
find /var/spool -xdev -printf '%h\n' | sort | uniq -c | sort -k1 -n | tail -20

# Delete millions of small files efficiently without hitting 'Argument list too long' errors
find /var/spool/clientmqueue -type f -delete

# Ultra-fast deletion technique for directories containing over 1 million files
rsync -a --delete /empty/dir/ /target/bloated/dir/

# Inspect total configured inode count on ext4 partition
tune2fs -l /dev/sda1 | grep -i 'inode count'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A 'No space left on device' error with free gigabytes indicates inode exhaustion, confirmed with `df -i`. Every file requires an inode; millions of micro-files consume the entire inode table. I isolate the culprit using `find / -xdev -printf '%h\n' | sort | uniq -c | sort -n`, purge the stale files using `find -type f -delete` or `rsync --delete` to avoid bash argument list overflow, and transition the workload to an XFS filesystem with dynamic inode allocation if such scale is routine."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 12 - Quick Revision Guide](../../23 - Real-World Projects & Enterprise Architectures/12-Project-Multi-Cloud-FinOps-Cost-Optimization-and-Governance/12-Quick-Revision.md) | [Index](../../../README.md) | [Linux & Systems Engineering Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

