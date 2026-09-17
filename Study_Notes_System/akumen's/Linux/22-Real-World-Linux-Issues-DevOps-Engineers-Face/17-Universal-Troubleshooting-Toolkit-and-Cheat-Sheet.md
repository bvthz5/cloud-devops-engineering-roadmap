# 17 — Universal Troubleshooting Toolkit and Cheat Sheet

## Essential Linux Diagnostic Command Matrix

| Layer / Resource | Primary Command | Key Options / Flags | Output / Diagnostic Purpose |
|---|---|---|---|
| **System Load** | `uptime` | (None) | 1, 5, 15 minute load averages |
| **CPU Usage** | `top` / `htop` | `-bn1` | Real-time CPU %us, %sy, %wa breakdown |
| **CPU Metrics** | `mpstat` | `-P ALL 1` | Per-core CPU utilization breakdown |
| **Process Stats** | `pidstat` | `-u 1 5` | Individual process CPU consumption over time |
| **Memory Allocation**| `free` | `-h` | Total, used, free, buffer/cache, swap RAM |
| **Paging & Swap** | `vmstat` | `1 5` | Memory swap in (`si`) / swap out (`so`) rates |
| **Disk Space** | `df` | `-h`, `-i` | Filesystem capacity & inode usage |
| **Directory Usage** | `du` | `-sh *` | Directory size calculation |
| **Open Files / Leaks**| `lsof` | `+L1`, `-p PID`, `-i :PORT` | Open file descriptors & deleted open file leaks |
| **Disk I/O** | `iostat` | `-xz 1` | Disk await time, IOPS (r/s, w/s), %util |
| **Process Disk I/O** | `iotop` | `-oPa` | Per-process disk I/O throughput |
| **Network Sockets** | `ss` | `-tulpn` | Listening sockets, established TCP connections |
| **DNS Resolution** | `dig` | `+trace domain`, `+short` | Authoritative DNS lookup chain |
| **Network Packet** | `tcpdump` | `-i eth0 -n port 80` | Real-time network packet capture |
| **System Logs** | `journalctl` | `-u unit -b -n 100 --no-pager` | Systemd service log investigation |
| **Kernel Log** | `dmesg` | `-T --level=err,warn` | Hardware & kernel ring buffer errors (OOM, disk) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Suspicious Processes](./16-Suspicious-Processes-and-Security-Incident-Response.md) | [README](./README.md) | [18 - Lab 01: Unlinked File Leak](./18-Hands-On-Lab-01-Unlinked-Open-File-Disk-Leak.md) |
