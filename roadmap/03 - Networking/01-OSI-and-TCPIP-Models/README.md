# Module 01: OSI and TCP/IP Networking Models

Understanding network abstraction models is the foundation of modern infrastructure engineering. Whether diagnosing a connection timeout between Kubernetes pods, configuring an AWS Application Load Balancer, or analyzing raw packet captures, engineers rely on the OSI and TCP/IP models to isolate problems layer by layer.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Dissect the **7-Layer OSI Reference Model** and understand the role of each layer.
- Master the **4-Layer TCP/IP Model** and compare it with the OSI model.
- Trace packet **Encapsulation and Decapsulation** across Protocol Data Units (PDUs).
- Differentiate **Layer 2 (Data Link / MAC)** switching from **Layer 3 (Network / IP)** routing.
- Compare **Layer 4 (Transport / TCP/UDP)** and **Layer 7 (Application / HTTP/TLS)** proxying and load balancing.
- Apply systematic layer-by-layer network troubleshooting methodologies in production.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [OSI 7-Layer Reference Model](./01-OSI-7-Layer-Reference-Model.md) | Physical to Application layers, PDUs, and architectural responsibilities | ✅ Complete |
| 02 | [TCP/IP 4-Layer Model & Comparison](./02-TCPIP-4-Layer-Model-and-Comparison.md) | Network Interface, Internet, Transport, Application, and real-world adoption | ✅ Complete |
| 03 | [Encapsulation & Decapsulation Data Flow](./03-Encapsulation-and-Decapsulation-Data-Flow.md) | Headers, trailers, MTU, packet assembly and disassembly walkthrough | ✅ Complete |
| 04 | [Layer 2 Data Link: MAC & Switching](./04-Layer-2-Data-Link-MAC-and-Switches.md) | Ethernet frames, MAC addresses, ARP protocol, and CAM switch tables | ✅ Complete |
| 05 | [Layer 3 Network: IP & Routing](./05-Layer-3-Network-IP-Routing-and-Routers.md) | IP packets, hop-by-hop routing, TTL decrement, and ICMP error reports | ✅ Complete |
| 06 | [Layer 4 vs Layer 7 in Cloud Infrastructure](./06-Layer-4-vs-Layer-7-Networking-in-Cloud.md) | AWS NLB vs ALB, Envoy proxying, TCP pass-through vs TLS termination | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Diagnosing multi-tier network partitions; MTU truncation postmortem | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Systematic bottom-up L1-L7 diagnostic checklist and commands | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical interview questions for DevOps, SRE, and Network Engineers | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Inspect packet encapsulation with tcpdump; view ARP tables | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | High-density layer reference table, protocol mappings, and PDU names | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Networking Master Index](../README.md) | [Networking Master Index](../README.md) | [01 - OSI 7-Layer Reference Model](./01-OSI-7-Layer-Reference-Model.md) |
