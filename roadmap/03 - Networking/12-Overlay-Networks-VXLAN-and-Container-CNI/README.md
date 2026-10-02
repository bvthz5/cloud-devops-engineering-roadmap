# Module 12: Overlay Networks, VXLAN, and Container CNI

Welcome to **Module 12: Overlay Networks, VXLAN, and Container CNI**. Containerization and Kubernetes abstracted physical infrastructure through software-defined overlay networking. Understanding packet encapsulation, CNI plugins, and MTU overhead is critical for every Cloud DevOps and Platform Engineer.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deeply understand the difference between **Underlay Networks** (physical/VPC fabric) and **Overlay Networks** (virtual encapsulated tunnels).
2. Deconstruct **VXLAN** (Virtual Extensible LAN) packet structure (24-bit VNI, UDP 4789) and **Geneve**.
3. Master the **Container Network Interface (CNI)** specification (`ADD`, `DEL`, `CHECK`) and Linux network namespaces (`veth` pairs).
4. Evaluate and compare production CNIs: **Flannel**, **Calico**, **Cilium (eBPF)**, and **AWS VPC CNI**.
5. Calculate encapsulation **MTU overhead** to prevent silent packet drops and TLS handshake freezes.
6. Implement zero-trust microsegmentation using **Kubernetes NetworkPolicies** and Cilium L7 DNS-aware policies.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Overlay Networking Principles](./01-Overlay-Networking-Principles-and-Encapsulation.md) | Underlay vs overlay, packet-in-packet encapsulation, L2 over L3 routing |
| 02 | [VXLAN & Geneve Protocol Deep Dive](./02-VXLAN-and-Geneve-Protocol-Deep-Dive.md) | VXLAN 24-bit VNI, UDP 4789, VTEP endpoints, Geneve TLV extensible headers |
| 03 | [CNI Specification & Architecture](./03-Container-Network-Interface-CNI-Architecture.md) | How kubelet calls CNI plugins, ADD/DEL actions, Linux netns and veth pairs |
| 04 | [Comparing Major CNIs](./04-Comparing-Major-CNIs-Flannel-Calico-Cilium-AWS-VPC-CNI.md) | Flannel vs Calico vs Cilium vs AWS VPC CNI trade-offs and benchmarks |
| 05 | [MTU Calculations & Clamping in Overlays](./05-MTU-Calculations-Overhead-and-Clamping-in-Overlays.md) | 50-byte encapsulation tax, MTU 1450 vs 1500, Path MTU discovery bugs |
| 06 | [NetworkPolicies & Microsegmentation](./06-Network-Policies-and-Micro-Segmentation.md) | Ingress/egress rules, Calico GlobalNetworkPolicy, Cilium L7 HTTP/DNS policies |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | AWS VPC CNI IP exhaustion outage, MTU drop freezing large database payloads |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | `ip netns` inspection, `cilium monitor`, finding peer veth interfaces |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Building a container network from scratch with Linux network namespaces and bridges |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | CNI comparison matrix, encapsulation overhead calculator, CNI CLI commands |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - BGP & Interconnects](../11-BGP-Routing-and-Cloud-Interconnects/README.md) | [Networking Index](../README.md) | [01 - Overlay Principles](./01-Overlay-Networking-Principles-and-Encapsulation.md) |
