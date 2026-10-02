# 05 - Kube-Proxy Modes: iptables vs IPVS vs Kernel Routing

## 1. Packet Processing Comparison

```text
IPTABLES MODE (Linear Netfilter Chains)
[Packet] ──► PREROUTING ──► KUBE-SERVICES ──► KUBE-SVC-XYZ ──► KUBE-SEP-ABC (Random 33%) ──► Pod IP
(Linear O(N) evaluation degrades CPU at high service counts)

IPVS MODE (IP Virtual Server - Hash Tables)
[Packet] ──► Linux IPVS Kernel Module ──► Hash Table Lookup [VIP:Port] ──► Pod IP
(Constant O(1) evaluation; scales effortlessly to 50k+ services)
```

---

## 2. eBPF Bypass (Cilium Kube-Proxy Replacement)

Modern clusters with **Cilium** remove `kube-proxy` entirely. Cilium attaches eBPF programs directly to the socket layer (`sock_ops`), rewriting packet destination IP before the packet even leaves userspace socket buffers, achieving near-native line rate speeds.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - CoreDNS Architecture](./04-CoreDNS-Architecture-and-Name-Resolution-Flow.md) | [README](./README.md) | [06 - EndpointSlices](./06-EndpointSlices-High-Scale-Service-Endpoints.md) |
