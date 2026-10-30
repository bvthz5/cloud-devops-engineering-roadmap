# 05 — Packet Analysis and Diagnostics: tcpdump, traceroute, and ping

When high-level tools fail, low-level packet sniffing and path analysis reveal whether packets are dropped by firewalls, corrupted by routing loops, or delayed by network congestion.

---

## 1. Deep Packet Inspection with `tcpdump`

`tcpdump` captures network packets passing through a network interface using the kernel Berkeley Packet Filter (BPF) engine.

### 1.1 Essential Flags
| Flag | Meaning | DevOps Purpose |
| :--- | :--- | :--- |
| `-i any` | Interface | Listen on all network interfaces simultaneously |
| `-nn` | No name resolution | Don't resolve hostnames or port names (prevents misleading DNS lookups) |
| `-v`, `-vv` | Verbose | Show detailed IP header, TTL, and protocol flags |
| `-s 0` | Snap length | Capture full packet payload without truncation |
| `-w capture.pcap` | Write to file | Save packet capture in standard PCAP format for Wireshark analysis |
| `-r capture.pcap` | Read from file | Replay and inspect captured PCAP file |
| `-c 100` | Count | Stop after capturing 100 packets |
| `-X` or `-XX` | Hex & ASCII | Print packet payload in hexadecimal and ASCII (read unencrypted headers) |

### 1.2 Powerful BPF Filter Expressions
```bash
# Capture only HTTP traffic on port 80 or 8080
sudo tcpdump -i any -nn 'port 80 or port 8080'

# Capture traffic to a specific host excluding SSH (port 22)
sudo tcpdump -i eth0 -nn 'host 10.0.1.50 and not port 22'

# Capture TCP SYN packets (detect new connection attempts or port scans)
sudo tcpdump -i any -nn 'tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0'

# Capture DNS requests and responses (UDP/TCP port 53)
sudo tcpdump -i any -nn -v 'port 53'
```

---

## 2. Path Tracing: `traceroute` vs `mtr`

### 2.1 How `traceroute` Works
`traceroute` sends packets with incrementing TTL (Time to Live) values starting from `TTL=1`. Each Layer 3 router decrements TTL by 1. When TTL reaches 0, the router discards the packet and sends back an `ICMP Time Exceeded` message, revealing the router's IP.

```bash
# Standard UDP traceroute
traceroute example.com

# ICMP traceroute (works better across firewalls that block UDP)
traceroute -I example.com

# TCP SYN traceroute to port 443 (proves whether transit routers block HTTPS)
sudo traceroute -T -p 443 example.com
```

### 2.2 `mtr` (My Traceroute - Dynamic Real-Time Diagnostic)
`mtr` combines `traceroute` and `ping` into a live, continuously updating diagnostic tool.
```bash
# Run interactive live MTR
mtr example.com

# Run non-interactive 10-packet report for incident postmortems
mtr --report --report-cycles 10 example.com
```

**Analyzing MTR Reports:**
```text
Host                           Loss%   Snt   Last   Avg  Best  Wrst StDev
1. 192.168.1.1                  0.0%    10    0.8   0.9   0.7   1.2   0.2
2. 10.240.0.1                   0.0%    10    1.5   1.8   1.4   2.6   0.4
3. 203.0.113.5                 60.0%    10   12.1  12.5  11.8  14.2   0.8  <-- ICMP rate limiting
4. 142.250.190.46               0.0%    10   14.2  14.4  14.1  15.1   0.3  <-- Final destination: 0% loss!
```
> [!NOTE]
> If packet loss appears on an intermediate hop (hop 3) but disappears on subsequent hops (hop 4), **it is not real network loss**. The intermediate router's control plane is simply rate-limiting ICMP responses.

---

## 3. Path MTU Discovery with `ping`

Test whether packets are being dropped due to MTU limitations using the "Don't Fragment" (DF) bit:

```bash
# Send 1472-byte payload + 28 bytes header = 1500 bytes MTU
ping -c 3 -M do -s 1472 8.8.8.8

# If MTU is too large for the path, you receive:
# ping: local error: message too long, mtu=1450
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - DNS Resolution Architecture and systemd resolved](./04-DNS-Resolution-Architecture-and-systemd-resolved.md) | [Index](../../../README.md) | [06 - Network Namespaces and Container Networking →](./06-Network-Namespaces-and-Container-Networking.md) |
