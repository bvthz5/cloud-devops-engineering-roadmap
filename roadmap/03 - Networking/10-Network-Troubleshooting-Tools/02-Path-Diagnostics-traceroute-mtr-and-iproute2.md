# 02 - Path Diagnostics: traceroute, mtr, and iproute2

## 1. How Traceroute Works

`traceroute` sends packets with incrementing **TTL (Time to Live)** values starting at 1. When an intermediate router receives a packet with `TTL=1`, it decrements it to 0, drops the packet, and sends back an `ICMP Time Exceeded` packet. The sender logs the router's IP and latency.

### Traceroute Probe Types:
- **UDP (Default on Linux):** Sent to high random UDP ports (33434+). Often blocked by modern cloud firewalls.
- **ICMP (`-I`):** Uses standard ping echo requests.
- **TCP SYN (`-T -p <port>`):** Probes using real TCP SYN packets (e.g. `-T -p 443`). **Best for bypassing enterprise firewalls!**

```bash
# Probe using TCP port 443
traceroute -T -p 443 -n api.github.com
```

---

## 2. MTR (My Traceroute): Continuous Path Analysis

`mtr` combines `traceroute` and `ping` into an interactive, real-time diagnostic console.

```bash
# Run 100 probes in non-interactive report mode and output to terminal
mtr --report --report-cycles 100 -n 8.8.8.8
```

### How to Correctly Interpret MTR Output:
```
HOST: bastion                     Loss%   Snt   Last   Avg  Best  Wrst StDev
  1.|-- 10.0.0.1                   0.0%   100    0.4   0.4   0.3   1.2   0.1
  2.|-- 198.51.100.1              60.0%   100    1.2   1.4   0.9  14.2   1.8  <-- ICMP Rate Limiting!
  3.|-- 203.0.113.50               0.0%   100   12.1  11.8  10.2  18.5   1.4
  4.|-- 8.8.8.8                    0.0%   100   14.2  14.0  12.8  22.1   1.2
```
**Golden Rule of MTR:** If packet loss at hop 2 does **not** persist into subsequent hops (3 and 4), **it is NOT real packet loss**! Intermediate routers simply deprioritize or rate-limit ICMP responses to save CPU.

---

## 3. Kernel Route Decision: `ip route get`
```bash
# Determine which interface, source IP, and gateway the kernel selects for a destination
ip route get 8.8.8.8
# Output: 8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.50 uid 1000
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Packet Capture](./01-Packet-Capture-tcpdump-and-Wireshark.md) | [README](./README.md) | [03 - Socket Inspection](./03-Socket-and-Connection-Inspection-ss-and-netstat.md) |
