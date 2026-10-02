# 01 - Kubernetes Networking Model and IP-per-Pod Rule

## 1. The Three Fundamental Tenets

The Kubernetes networking model enforces three strict rules:
1. **All Pods can communicate with all other Pods without NAT.**
2. **All Nodes can communicate with all Pods (and vice-versa) without NAT.**
3. **The IP that a Pod sees as its own IP is the exact same IP that all other Pods see for it.**

```text
+-----------------------+                         +-----------------------+
| Worker Node 01        |                         | Worker Node 02        |
| Host IP: 192.168.1.10 |                         | Host IP: 192.168.1.11 |
| Pod CIDR: 10.244.1.0/24                         | Pod CIDR: 10.244.2.0/24
|                       |                         |                       |
|   [ Pod A: 10.244.1.5 ]──────Direct IP Route────►[ Pod B: 10.244.2.8 ]  |
|                       |     (No Masquerading)   |                       |
+-----------------------+                         +-----------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - CNI Specification](./02-Container-Network-Interface-CNI-Specification.md) |
