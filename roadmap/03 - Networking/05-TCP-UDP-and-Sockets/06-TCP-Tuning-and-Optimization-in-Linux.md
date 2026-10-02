# 06 — TCP Kernel Tuning and Optimization in Linux

Tune these parameters in `/etc/sysctl.d/99-tcp.conf` for high-throughput cloud workloads:

```ini
# Reuse TIME_WAIT sockets for outbound connections
net.ipv4.tcp_tw_reuse = 1

# Increase system-wide socket listen backlog
net.core.somaxconn = 65535

# Increase SYN backlog for incoming connection bursts
net.ipv4.tcp_max_syn_backlog = 16384

# Expand ephemeral port range
net.ipv4.ip_local_port_range = 10240 65535

# Enable BBR congestion control (Requires Linux 4.9+)
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

Apply immediately:
```bash
sudo sysctl --system
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Network Sockets & BSD API](./05-Network-Sockets-and-the-BSD-Socket-API.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
