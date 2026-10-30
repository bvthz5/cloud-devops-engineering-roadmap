# 04 - Comparing Major CNIs: Flannel, Calico, Cilium, AWS VPC CNI

## 1. Architectural Comparison Matrix

| Feature | Flannel | Project Calico | Cilium | AWS VPC CNI |
|---|---|---|---|---|
| **Data Plane** | Linux Bridge / VXLAN | iptables or eBPF | **eBPF (Kernel Bypass)** | AWS VPC Native ENIs |
| **Routing Mode** | VXLAN or host-gw | BGP or VXLAN | eBPF Direct Routing | Direct VPC Routing |
| **Network Policies** | ❌ None | ✅ L3/L4 Policies | ✅ **L3/L4 + L7 (HTTP/gRPC/DNS)** | Security Groups for Pods |
| **Performance** | Baseline | High | **Extreme (Zero-iptables overhead)** | Wire speed (Native AWS fabric) |
| **IP Allocation** | Subnet per node | IPAM IP pools | Dynamic CIDR / IPAM | VPC Subnet IP exhaustion risk |
| **Observability** | Minimal | Basic | **Hubble (Flow logs, DNS, latency)** | AWS VPC Flow Logs |

---

## 2. Deep Dive Recommendations
- **Small On-Prem / HomeLab:** Flannel (`host-gw` mode) for dead-simple setup.
- **Enterprise Kubernetes (Multi-Cloud / Hybrid):** **Cilium** or **Calico**.
- **AWS EKS Enterprise:** AWS VPC CNI if you require native VPC Security Groups and direct routing; Cilium chained if you need L7 observability and policy enforcement.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Container Network Interface CNI Architecture](./03-Container-Network-Interface-CNI-Architecture.md) | [Index](../../../README.md) | [05 - MTU Calculations Overhead and Clamping in Overlays →](./05-MTU-Calculations-Overhead-and-Clamping-in-Overlays.md) |
