# 02 - Container Network Interface (CNI) Specification

## 1. The CNI Plugin Contract

The **Container Network Interface (CNI)** is a CNCF project that standardizes network configuration for Linux containers.
When a Pod is scheduled, `kubelet` calls the configured CNI binary:
- **`ADD`:** Creates a `veth` pair, moves one end into the Pod's network namespace as `eth0`, and assigns an IP via IPAM.
- **`DEL`:** Reclaims IP address and tears down the virtual ethernet interface.
- **`CHECK`:** Verifies that the pod network interface is operational.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Networking Model](./01-Kubernetes-Networking-Model-and-IP-per-Pod-Rule.md) | [README](./README.md) | [03 - Calico CNI & BGP](./03-Calico-CNI-BGP-Routing-and-IP-Pools.md) |
