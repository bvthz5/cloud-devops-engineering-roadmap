# 08 — Linux Network Troubleshooting Guide & Runbook

When diagnosing network outages, follow a structured, bottom-up approach following the OSI model (Layer 1 Physical to Layer 7 Application) to avoid guessing.

---

## 1. The Systematic 7-Step Diagnostic Flowchart

```text
[Step 1: Layer 1 & 2 Physical / Link]
Is the interface UP and receiving carrier signal?
-> ip link show dev eth0
   Look for: <UP,LOWER_UP>, speed, duplex via ethtool eth0.
   Look for: dropped packets, collisions via ip -s link.

[Step 2: Layer 3 IP Configuration]
Does the interface have a valid IP and subnet mask?
-> ip -4 addr show dev eth0

[Step 3: Layer 3 Gateway & Routing]
Can the host reach its default gateway?
-> ip route show (Verify default via ...)
-> ping -c 3 <gateway-ip>
-> ip neigh show (Verify gateway MAC address resolved)

[Step 4: Path & Firewalls]
Is external routing working and are packets passing intermediate hops?
-> ping -c 3 8.8.8.8
-> traceroute -n 8.8.8.8 or mtr 8.8.8.8
-> sudo iptables -L -n -v (Check DROP counters)

[Step 5: Layer 4 Ports & Sockets]
Is the target service actually listening and accepting connections?
-> sudo ss -tulpn | grep <port>
-> Check Recv-Q vs Send-Q

[Step 6: Layer 7 DNS Resolution]
Can the system resolve hostnames to IP addresses?
-> dig +short example.com
-> resolvectl status
-> cat /etc/resolv.conf

[Step 7: Application & TLS]
Is the application layer responding?
-> curl -Iv https://example.com
```

---

## 2. Emergency Triage Runbook

| Symptom | Diagnostic Command | Likely Root Cause & Fix |
| :--- | :--- | :--- |
| **"Network is unreachable"** | `ip route show` | Missing default gateway route. Fix: `sudo ip route add default via <gw-ip> dev eth0`. |
| **"Connection refused"** | `sudo ss -tulpn \| grep <port>` | Service is not running or listening on `127.0.0.1` instead of `0.0.0.0`. Fix service bind address. |
| **"Connection timed out"** | `sudo iptables -S` or security group check | Security Group, cloud firewall, or local `iptables`/`ufw` dropping packets silently. |
| **"Temporary failure in name resolution"** | `resolvectl status` or `dig @8.8.8.8 google.com` | DNS failure. Upstream nameserver unreachable or `/etc/resolv.conf` misconfigured. |
| **"Cannot assign requested address"** | `ss -s` and `cat /proc/sys/net/ipv4/ip_local_port_range` | Ephemeral port exhaustion due to thousands of `TIME_WAIT` sockets. Enable `tcp_tw_reuse`. |
| **Intermittent large packet drops** | `ping -M do -s 1472 <dest>` | MTU mismatch. Lower interface MTU to match path MTU. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
