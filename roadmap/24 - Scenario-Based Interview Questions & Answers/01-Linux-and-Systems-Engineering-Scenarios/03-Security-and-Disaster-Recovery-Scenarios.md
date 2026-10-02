# Linux & Systems Engineering Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Network Packet Drops at the OS Kernel Level Under Heavy Load

### 🚨 The Production Scenario
An e-commerce API server experiences intermittent 5-second connection delays during flash sales. Cloud metrics show CPU at 60%, but clients receive connection resets.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
High packet volume can overwhelm the network interface card (NIC) ring buffers and OS socket receive queues. If the kernel's `netdev_max_backlog` or TCP listen backlog (`somaxconn`) is too small, incoming SYN packets are silently dropped, triggering TCP exponential backoff retransmissions.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check interface-level packet drop statistics using `ethtool -S` and `netstat -s`.
- Step 2: Check for TCP backlog drops using `netstat -s | grep -i listen`.
- Step 3: Increase the NIC ring buffer size using `ethtool -G`.
- Step 4: Tune kernel network backlog parameters (`net.core.netdev_max_backlog`, `net.core.somaxconn`).
- Step 5: Enable TCP SYN cookies and optimize TCP socket buffers in `/etc/sysctl.conf`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect TCP socket listen queue drops and buffer overflows
netstat -s | grep -iE 'listen|overflowed|dropped'

# Display current and maximum supported NIC ring buffer RX/TX sizes
ethtool -g eth0

# Maximize NIC RX and TX ring buffers to absorb bursty traffic spikes
sudo ethtool -G eth0 rx 4096 tx 4096

# Increase maximum listen backlog queue for connection handshakes
sysctl -w net.core.somaxconn=65535

# Increase kernel input packet queue for high-bandwidth NICs
sysctl -w net.core.netdev_max_backlog=10000

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Kernel packet drops under moderate CPU indicate queue saturation at the driver or socket layer. I run `netstat -s | grep listen` to check for listen backlog overflows and `ethtool -S` for ring buffer drops. I remediate by scaling NIC ring buffers with `ethtool -G rx 4096`, increasing `net.core.somaxconn` and `netdev_max_backlog`, and ensuring the application server's accept backlog matches the kernel limit."

---

## 📌 Scenario 8: SSH Access Fails with 'Connection Closed by Remote Host' during Production Outage

### 🚨 The Production Scenario
During an emergency incident, engineers cannot SSH into an Ubuntu production bastion or web nodes. SSH connections immediately terminate with `Connection reset by peer` or `Connection closed`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Common root causes include: 1) SSH daemon process limits reached (`MaxStartups` dropping unauthenticated connections), 2) `/etc/security/limits.conf` or PAM session limits exhausted, 3) Incorrect file permissions on `~/.ssh` or `authorized_keys`, or 4) iptables/fail2ban ban triggering after multiple parallel monitoring checks.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Test connection with verbose debug output: `ssh -vvv user@server`.
- Step 2: Access via Cloud Serial Console (AWS EC2 Serial Console or Azure Serial Console).
- Step 3: Review `/var/log/auth.log` or `journalctl -u sshd -e` for authentication denials.
- Step 4: Check if `sshd` MaxStartups is throttling concurrent connection attempts.
- Step 5: Verify permissions: `chmod 700 ~/.ssh` and `chmod 600 ~/.ssh/authorized_keys`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Run SSH client with maximum verbosity to pinpoint handshake failure stage
ssh -vvv -o ConnectTimeout=10 user@server.domain.com

# Inspect remote SSH server authentication and session logs
sudo journalctl -u sshd -n 100 --no-pager

# Check if bastion IP was accidentally jailed by automated brute-force protection
sudo fail2ban-client status sshd

# Verify SSH concurrent unauthenticated connection throttling configuration
grep -i maxstartups /etc/ssh/sshd_config

# Verify firewall packet counters on port 22 to confirm packet arrival
sudo iptables -L -n -v | grep 22

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "I troubleshoot SSH drops systematically starting from `ssh -vvv` to determine whether failure occurs at TCP connect, key exchange, or PAM session creation. If blocked, I access via cloud Out-of-Band Serial Console, inspect `journalctl -u sshd`, and verify `MaxStartups` (which throttles burst connections during outages) and fail2ban jails. I also verify that `~/.ssh` permissions are strictly 700 and `authorized_keys` is 600."

---

## 📌 Scenario 9: Read-Only Filesystem Remount on Production Storage Array

### 🚨 The Production Scenario
Application logs suddenly throw `Read-only file system` errors on `/dev/sdb1`. No files can be updated or created.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Filesystem drivers (like ext4) are configured by default in `/etc/fstab` with `errors=remount-ro`. When the kernel detects underlying disk corruption, SCSI I/O timeouts, or SAN/EBS detachment errors, it immediately remounts the filesystem as read-only to prevent catastrophic data corruption.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check `dmesg -T` and `journalctl -k` for SCSI I/O errors or filesystem journal aborts.
- Step 2: Verify current mount options with `mount | grep sdb1`.
- Step 3: Check underlying storage connectivity (e.g. AWS EBS volume state, SAN link).
- Step 4: Safely unmount the device: `umount /mnt/data` (or lazy unmount if busy).
- Step 5: Run a non-destructive filesystem consistency check (`fsck -y /dev/sdb1`) and remount read-write.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check kernel buffer for storage hardware errors and read-only remount triggers
dmesg -T | grep -iE 'ext4|xfs|io error|remount'

# Display all currently mounted filesystems that are operating in read-only mode
mount | grep 'ro,'

# Force consistency check and repair filesystem errors on unmounted volume
sudo fsck -f -y /dev/sdb1

# Attempt to remount volume in read-write mode once disk errors are resolved
sudo mount -o remount,rw /mnt/data

# Inspect S.M.A.R.T. hardware health indicators on physical disk
smartctl -H /dev/sdb

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Linux remounts filesystems as read-only as a self-preservation mechanism (`errors=remount-ro`) when it detects block corruption or storage controller disconnects. I check `dmesg` to verify whether it was an transient I/O timeout or real sector corruption, unmount the volume, perform an `fsck` repair, verify hardware/EBS health, and remount read-write."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Linux & Systems Engineering Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Linux & Systems Engineering Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

