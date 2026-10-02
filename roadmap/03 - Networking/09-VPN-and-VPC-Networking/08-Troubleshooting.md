# 08 - VPN & VPC: Troubleshooting Guide

## 1. Asymmetric Routing over Hybrid VPNs

### The Problem
Traffic leaves an on-prem server via Direct Connect, but return traffic from AWS chooses an IPsec VPN tunnel due to differing BGP AS-Path metrics. Stateful firewalls on both sides drop the packets because they only see half of the TCP handshake!

### Fix: BGP AS-Path Prepending & Local Preference
Ensure identical primary and backup routing policies:
- In AWS, configure Direct Connect routes with higher BGP Local Preference.
- Prepend AS-Paths on the backup IPsec tunnel so returning packets consistently follow the Direct Connect path.

---

## 2. MTU / MSS Clamping Blackholes over IPsec
IPsec encapsulation adds 50-70 bytes of header overhead. If an endpoint transmits packets with standard Ethernet MTU 1500 and `DF` (Don't Fragment) bit set, intermediate routers drop the packet. If ICMP `Destination Unreachable (Fragmentation Needed)` is blocked by firewalls, connections hang indefinitely after the initial TLS Client Hello!

### The Fix: TCP MSS Clamping on VPN Routers
```bash
# Clamp TCP MSS to 1360 bytes on the VPN interface
sudo iptables -t mangle -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
