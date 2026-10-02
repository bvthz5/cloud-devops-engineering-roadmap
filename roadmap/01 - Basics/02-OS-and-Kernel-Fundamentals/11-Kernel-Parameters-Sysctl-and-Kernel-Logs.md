# 11 — Kernel Parameters, Sysctl Tuning, and Kernel Ring Buffer Logs

---

## 1. Runtime Kernel Parameters: The `/proc/sys/` Interface

The Linux kernel does not require a recompile or system reboot to alter most of its runtime operational parameters. The kernel exposes its internal configuration parameters directly to user space through the virtual filesystem mounted at **`/proc/sys/`**.

```text
Kernel Internal Settings (TCP window, swap aggressiveness, file limits)
                               ▲
                               │
               Exposed via /proc/sys/ Virtual Tree
                               │
       ┌───────────────────────┼───────────────────────┐
       ▼                       ▼                       ▼
  /proc/sys/fs/           /proc/sys/net/          /proc/sys/vm/
  (Filesystem limits)     (Networking & TCP)      (Virtual Memory & Swap)
```

Reading or writing to these virtual files directly interacts with running kernel memory:
```bash
# Check current TCP maximum connection queue length
cat /proc/sys/net/core/somaxconn

# Modify runtime parameter directly using echo
sudo sh -c 'echo 4096 > /proc/sys/net/core/somaxconn'
```

---

## 2. Managing Kernel Parameters with `sysctl`

The **`sysctl`** utility is the standard administrative tool used to query and modify runtime kernel parameters. Slashing `/` paths in `/proc/sys/` are translated into dot notation:
`/proc/sys/net/ipv4/ip_forward` becomes `net.ipv4.ip_forward`.

### Core `sysctl` Commands
```bash
# 1. Display all active kernel parameters (~1500+ parameters)
sudo sysctl -a | head -n 25

# 2. Query a specific parameter
sysctl net.ipv4.ip_forward

# 3. Modify a parameter at runtime (active immediately, lost on reboot)
sudo sysctl -w net.ipv4.ip_forward=1

# 4. Reload persistent configuration from /etc/sysctl.conf and /etc/sysctl.d/
sudo sysctl -p /etc/sysctl.d/99-kubernetes.conf
```

---

## 3. Production Kernel Parameter Tuning Guide for DevOps & SREs

Below are the most critical kernel parameters tuned in high-concurrency environments, Kubernetes clusters, and low-latency databases:

| Parameter | Default | Production Value | Purpose / Impact |
| :--- | :--- | :--- | :--- |
| **`net.core.somaxconn`** | `128` or `4096` | `65535` | Maximum socket listen backlog for pending TCP connections. Prevents `connection refused` bursts on Nginx / Redis. |
| **`net.ipv4.tcp_max_syn_backlog`**| `512` | `3240000` | Size of the queue holding half-open TCP connections (SYN received, waiting for ACK). Mitigates SYN flood attacks. |
| **`net.ipv4.ip_forward`** | `0` | `1` | Enables packet routing across interfaces. **Mandatory for Kubernetes CNI (Calico, Flannel, Cilium)** and Docker. |
| **`fs.file-max`** | ~`100000` | `2097152` | System-wide ceiling for open file descriptors across all processes. |
| **`vm.swappiness`** | `60` | `10` or `1` | Reduces kernel aggressiveness when swapping application RAM to disk; prevents database latency spikes. |
| **`vm.overcommit_memory`**| `0` | `1` | When set to `1`, kernel allows heuristic memory overcommit. **Mandatory for Redis** background RDB snapshots. |
| **`fs.inotify.max_user_watches`**| `8192` | `524288` | Number of files a user can monitor using inotify. Essential for Kubernetes nodes running hundreds of pods. |

### Enterprise Kubernetes Configuration Example
Save to `/etc/sysctl.d/99-k8s-node.conf`:
```ini
# Enable IPv4 packet forwarding for Pod network routing
net.ipv4.ip_forward = 1

# Bridge netfilter packet filtering across iptables
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1

# High concurrency networking queue depths
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 3240000

# File descriptor and inotify ceilings
fs.file-max = 2097152
fs.inotify.max_user_watches = 524288
fs.inotify.max_user_instances = 8192

# Memory stability for container workloads
vm.overcommit_memory = 1
vm.swappiness = 10
```

---

## 4. Kernel Logging & The Ring Buffer (`dmesg`)

The operating system kernel does not write its log messages directly to disk files using standard I/O (because storage drivers or filesystems might not yet be loaded or could be corrupted).

Instead, the kernel logs messages to an in-memory circular buffer known as the **Kernel Ring Buffer**.

```text
Kernel Subsystems (Hardware, OOM, Network, Drivers)
                     │
                     ▼ printk()
┌────────────────────────────────────────────────────────┐
│             Kernel In-Memory Ring Buffer               │
│ [Oldest Logs] <── Overwritten when full ── [Newest Logs]│
└──────────────────────────┬─────────────────────────────┘
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
      dmesg CLI Tool            systemd-journald / rsyslog
   (Dumps buffer directly)       (Persists to /var/log/kern.log)
```

---

## 5. Working with `dmesg` in Production

```bash
# 1. Print kernel messages with human-readable timestamps
dmesg -T | tail -n 25

# 2. Filter logs by severity level (err, crit, alert, emerg)
dmesg -T -l err,crit,alert,emerg

# 3. Follow kernel logs in real-time (live tail)
dmesg -Tw

# 4. Search specifically for Out-Of-Memory (OOM) killer events
dmesg -T | grep -i "out of memory"

# 5. Search for storage/disk errors
dmesg -T | grep -E "nvme|sda|EXT4-fs error|I/O error"
```

### Log Levels in the Kernel
Kernel log entries use syslog priority levels (from `0` emergency to `7` debug):
- `0 (emerg)`: System is unusable (Kernel panic).
- `1 (alert)`: Action must be taken immediately.
- `2 (crit)`: Critical hardware or software condition.
- `3 (err)`: Error conditions (e.g., driver probe failure).
- `4 (warn)`: Warning conditions (e.g., thermal limit reached).
- `5 (notice)`: Normal but significant condition.
- `6 (info)`: Informational messages (device detections).
- `7 (debug)`: Debug-level messages.
