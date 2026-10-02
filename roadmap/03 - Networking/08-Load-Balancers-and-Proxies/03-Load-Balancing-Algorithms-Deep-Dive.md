# 03 - Load Balancing Algorithms Deep Dive

## 1. Core Algorithms Overview

| Algorithm | How it Works | Best Used For | Drawbacks |
|---|---|---|---|
| **Round Robin** | Requests distributed sequentially: `A -> B -> C -> A` | Homogeneous servers with equal capacity and uniform request runtimes | Does not account for server capacity or long-lived requests |
| **Weighted Round Robin** | Servers with higher weight receive proportionally more requests | Heterogeneous hardware (e.g. 16-core vs 4-core VM) | Fixed ratios, does not react to dynamic latency |
| **Least Connections** | Forwards request to server with lowest active connection count | Long-running connections (WebSockets, database pools) | New connections might flood a recently recovered server |
| **IP Hash** | Hashes Client IP: `hash(ClientIP) % N` | Basic session stickiness without cookies | Large corporate proxies funnel thousands of users to 1 backend |
| **Consistent Hashing** | Uses a circular hash ring (Ketama) to map keys to servers | Distributed caches (Memcached, Redis), stateful microservices | Complex node rebalancing ring logic |

---

## 2. Consistent Hashing (Ketama Algorithm)

In standard modulo hashing (`hash(key) % N`), when 1 node out of 10 fails, **90% of all keys are remapped**, causing a catastrophic cache stampede or database meltdown.

In **Consistent Hashing**:
1. Keys and servers are mapped to a 360-degree hash ring ($0$ to $2^{32}-1$).
2. A request key is placed on the ring and moves clockwise until it hits the first server.
3. When a server node is added or removed, **only $1/N$ of the keys are moved**!

```
                    [Server A (vnode 1)]
                         /        \
                        /          \
           [Key 1]     /            \    [Server B (vnode 1)]
               │      /              \       │
               └───► O                O ◄─────┘
                    /                  \
                   /                    \
       [Server C] O                      O [Server A (vnode 2)]
                   \                    /
                    \                  /
                     O────────────────O
              [Server B (vnode 2)]  [Key 2]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Reverse Proxies Forward Proxies and Gateways](./02-Reverse-Proxies-Forward-Proxies-and-Gateways.md) | [Index](../../../README.md) | [04 - Health Checks Flapping and Graceful Drain →](./04-Health-Checks-Flapping-and-Graceful-Drain.md) |
