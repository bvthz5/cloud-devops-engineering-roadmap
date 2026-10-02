# Module 11: BGP Routing, Autonomous Systems, and Cloud Interconnects

Welcome to **Module 11: BGP Routing, Autonomous Systems, and Cloud Interconnects**. Border Gateway Protocol (BGP) is the fundamental routing protocol of the global internet, hybrid cloud enterprise backbones, and modern bare-metal Kubernetes container networking.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Master **BGP architecture**, Autonomous System Numbers (**ASNs**), and the distinction between **eBGP** and **iBGP**.
2. Decipher the exact 8-step **BGP Path Selection Algorithm** (Weight, Local Preference, AS-Path, Origin, MED).
3. Architect enterprise hybrid cloud links using **AWS Direct Connect**, **Azure ExpressRoute**, and **GCP Cloud Interconnect**.
4. Configure **BGP control planes in Kubernetes** using **Project Calico** and **MetalLB** for bare-metal LoadBalancer routing.
5. Mitigate route hijacking and security leaks using **RPKI** (Resource Public Key Infrastructure) and Route Origin Authorizations (ROAs).
6. Troubleshoot BGP neighbor state machines (`Idle` -> `Active` -> `Established`) and session drops.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [BGP Architecture & Autonomous Systems](./01-BGP-Fundamentals-Autonomous-Systems-and-eBGP-vs-iBGP.md) | AS numbers, path vector protocol, eBGP vs iBGP peering rules |
| 02 | [BGP Path Selection Attributes](./02-BGP-Path-Selection-Attributes-and-Metrics.md) | Weight, Local Pref, AS-Path prepending, MED, step-by-step route decision |
| 03 | [Cloud Dedicated Interconnects](./03-Cloud-Dedicated-Interconnects-DirectConnect-and-ExpressRoute.md) | AWS Direct Connect, Azure ExpressRoute, cross-connects, private VIFs |
| 04 | [Kubernetes BGP: Calico & MetalLB](./04-BGP-Control-Plane-in-Kubernetes-Calico-and-MetalLB.md) | Pod CIDR advertisement to Top-of-Rack (ToR) switches, BGP unnumbered |
| 05 | [BGP Security & RPKI Validation](./05-BGP-Security-RPKI-Route-Hijacking-and-Flap-Damping.md) | Route leaks, prefix hijacking defense, cryptographic ROAs, route flap damping |
| 06 | [Dynamic Routing: OSPF vs. BGP](./06-Dynamic-Routing-Protocols-OSPF-vs-BGP.md) | Interior Gateway Protocol (IGP) link-state vs Exterior Gateway Protocol (EGP) |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | The global Facebook BGP withdrawal outage, asymmetric Direct Connect flap |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | BGP finite state machine, BGP Active state debugging, MTU mismatch drops |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Establishing an eBGP session between two Linux routers using FRRouting (FRR) |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | BGP attribute priority cheat sheet, state machine reference, CLI commands |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Troubleshooting Tools](../10-Network-Troubleshooting-Tools/README.md) | [Networking Index](../README.md) | [01 - BGP Architecture](./01-BGP-Fundamentals-Autonomous-Systems-and-eBGP-vs-iBGP.md) |
