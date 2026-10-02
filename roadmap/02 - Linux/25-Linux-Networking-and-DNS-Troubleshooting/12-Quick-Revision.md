# 12 — Quick Revision Cheat Sheet: Linux Networking & DNS

A rapid-reference summary of commands, configurations, and concepts.

---

## 1. Network Command Translation Cheat Sheet

| Task | Legacy Command | Modern `iproute2` / `ss` Command |
| :--- | :--- | :--- |
| **Show interfaces** | `ifconfig -a` | `ip link show` |
| **Show IP addresses** | `ifconfig` | `ip -4 addr show` |
| **Add IP address** | `ifconfig eth0:1 10.0.0.5` | `sudo ip addr add 10.0.0.5/24 dev eth0` |
| **Show routing table**| `route -n` or `netstat -rn` | `ip route show` |
| **Add default gateway**| `route add default gw 192.168.1.1`| `sudo ip route add default via 192.168.1.1 dev eth0` |
| **Show listening ports**| `netstat -tulpn` | `sudo ss -tulpn` |
| **Show ARP table** | `arp -n` | `ip neigh show` |
| **Trace route** | `traceroute host` | `mtr --report host` or `ip route get ip` |
| **Query DNS** | `nslookup host` | `dig +short host` or `resolvectl query host` |

---

## 2. Key Network Files

- `/etc/resolv.conf`: Upstream nameservers and search domains.
- `/etc/nsswitch.conf`: Name Service Switch database lookup priority.
- `/etc/hosts`: Local static hostname-to-IP mappings.
- `/sys/class/net/`: Virtual filesystem exposing hardware and virtual network interfaces.
- `/proc/sys/net/ipv4/`: Runtime tunable kernel network stack parameters.

---

## 3. High-Priority Kernel Network Parameters (`sysctl`)

```ini
# Enable IP packet forwarding (required for routers and container hosts)
net.ipv4.ip_forward = 1

# Reuse TIME_WAIT sockets for outgoing connections
net.ipv4.tcp_tw_reuse = 1

# Increase listen backlog for high-concurrency web servers
net.core.somaxconn = 4096

# Increase ephemeral port range
net.ipv4.ip_local_port_range = 10240 65535

# Increase socket read/write buffer maximums
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple Choice Questions](./11-MCQ.md) | [README](./README.md) | [Next Module: 26 - Linux Firewalls](../26-Linux-Firewalls-iptables-nftables-and-UFW/README.md) |
