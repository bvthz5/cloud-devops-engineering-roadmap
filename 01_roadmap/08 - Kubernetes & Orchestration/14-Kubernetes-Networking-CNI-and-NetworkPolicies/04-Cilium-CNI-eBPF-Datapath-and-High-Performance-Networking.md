# 04 - Cilium CNI: eBPF Datapath and High-Performance Networking

## 1. The eBPF Datapath Revolution

Traditional CNI plugins route packets through heavy Linux Netfilter/iptables chains.
**Cilium** replaces iptables with **Extended Berkeley Packet Filter (eBPF)** programs loaded directly into the Linux kernel:
- Bypasses IP routing tables and iptables connection tracking.
- Performs routing directly at the Linux socket layer (`sock_ops`).
- Eliminates `kube-proxy` entirely.
- Includes **Hubble**: Deep Layer 7 network flow observability (HTTP status codes, latency, DNS tracing).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Calico CNI BGP Routing and IP Pools](./03-Calico-CNI-BGP-Routing-and-IP-Pools.md) | [Index](../../../README.md) | [05 - Native NetworkPolicies Ingress Egress and Namespaces →](./05-Native-NetworkPolicies-Ingress-Egress-and-Namespaces.md) |
