# 08 - BGP Routing: Troubleshooting Guide

## 1. BGP Finite State Machine (FSM)

```
[ Idle ] ──► [ Connect ] ──► [ OpenSent ] ──► [ OpenConfirm ] ──► [ Established ]
   ▲               │                                                      │
   │               ▼                                                      ▼
   └───────── [ Active ] ◄──────────────────────────────────────── All Normal Traffic
```

### Critical State Troubleshooting:
- **Stuck in `Idle`:** Router has no IP route to reach the BGP neighbor's peering IP address, or administrative shutdown.
- **Stuck in `Active`:** The router is actively trying to establish a TCP connection on port 179, but the TCP 3-way handshake is failing!
  - *Causes:* Firewall blocking TCP port 179, incorrect neighbor IP, or eBGP multihop TTL expired.
- **`Established`:** Peering successful. Prefix exchange active.

---

## 2. Essential BGP Diagnostic Commands

```bash
# In FRR / Cisco / Arista CLI:
show ip bgp summary
show ip bgp neighbors 192.168.1.1
show ip bgp 10.0.0.0/16
show ip bgp advertised-routes
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
