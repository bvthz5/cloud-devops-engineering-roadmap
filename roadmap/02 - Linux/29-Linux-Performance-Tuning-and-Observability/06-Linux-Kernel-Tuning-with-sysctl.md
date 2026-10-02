# 06 — Linux Kernel Tuning with sysctl and Limits

Production cloud servers running high-concurrency microservices, databases, or Kafka clusters must be tuned beyond conservative Linux distribution defaults.

---

## 1. File Descriptor Limits (`Too many open files`)

In Linux, every network socket, open file, and pipe consumes a file descriptor.
Default per-process limit is often 1024, causing high-traffic servers to fail with `Too many open files`.

### System-Wide Limit (`/etc/sysctl.d/99-limits.conf`):
```ini
fs.file-max = 2097152
```

### Per-User Limit (`/etc/security/limits.d/99-app.conf`):
```text
*       soft    nofile    65536
*       hard    nofile    65536
*       soft    nproc     65536
*       hard    nproc     65536
```

---

## 2. Production High-Performance Kernel Profile

Create `/etc/sysctl.d/99-performance.conf`:
```ini
# Network Socket Listen Backlog
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 16384

# Reclaim TIME_WAIT sockets
net.ipv4.tcp_tw_reuse = 1

# Ephemeral Port Range Expansion
net.ipv4.ip_local_port_range = 10240 65535

# Virtual Memory: Lower Swappiness & Tune Dirty Page Flushing
vm.swappiness = 10
vm.dirty_background_ratio = 5
vm.dirty_ratio = 10

# Increase maximum memory map areas (Required for Elasticsearch)
vm.max_map_count = 262144
```

Apply immediately:
```bash
sudo sysctl --system
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Historical Monitoring](./05-Historical-System-Activity-Monitoring-sysstat-sar.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
