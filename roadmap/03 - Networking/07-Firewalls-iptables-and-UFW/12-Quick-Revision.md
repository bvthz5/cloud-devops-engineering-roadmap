# 12 - Firewalls & iptables: Quick Revision Cheat Sheet

## Table & Hook Evaluation Summary

```
Packet Arrival
  ↓
[PREROUTING] ──► raw → mangle → nat (DNAT)
  ↓
[Routing Decision]
  ├── Local IP? ──► [INPUT] ──► mangle → filter → security ──► Local App
  └── Remote IP? ─► [FORWARD] ─► mangle → filter → security ──► [POSTROUTING]
                                                                     │
[OUTPUT] ──────► raw → mangle → nat → filter → security ─────────────┤
  ↓                                                                  ↓
Local App Out                                                mangle → nat (SNAT)
                                                                     ↓
                                                                Exit via NIC
```

## Essential CLI Cheat Sheet

| Command | Action |
|---|---|
| `iptables -vnL --line-numbers` | List all filter rules with packet counters |
| `iptables -t nat -vnL` | List NAT rules |
| `iptables -F` | Flush all rules in default table (DANGER!) |
| `conntrack -L` | View active connection tracking entries |
| `nft list ruleset` | View all active nftables rules |
| `ufw status numbered` | View numbered UFW rules |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (08-Load-Balancers-and-Proxies) →](../08-Load-Balancers-and-Proxies/01-Layer-4-vs-Layer-7-Load-Balancing.md) |
