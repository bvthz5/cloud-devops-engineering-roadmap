# 02 - AKS Networking: Kubenet vs Azure CNI

- **Kubenet**: Pods receive IP addresses from a logical overlay network. Requires smaller VNet CIDR space.
- **Azure CNI**: Every Pod receives a real, native IP address directly from the Azure VNet subnet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AKS Architecture Managed Control Plane and Node Pools](./01-AKS-Architecture-Managed-Control-Plane-and-Node-Pools.md) | [Index](../../../README.md) | [03 - AKS Authentication Entra ID Integration and Workload Identity →](./03-AKS-Authentication-Entra-ID-Integration-and-Workload-Identity.md) |
