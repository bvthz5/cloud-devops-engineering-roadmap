# Module 25: Linux Networking and DNS Troubleshooting

Linux networking forms the fundamental bedrock of modern cloud computing, microservices, container runtimes (Docker, Kubernetes), and hybrid cloud architectures. Understanding how the Linux kernel handles packets, routes traffic, manages sockets, and resolves domain names is an indispensable superpower for DevOps Engineers and Site Reliability Engineers (SREs).

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand the Linux kernel network stack, network device abstraction, and interface types (`eth`, `lo`, `veth`, `br0`).
- Master modern `iproute2` tooling (`ip link`, `ip addr`, `ip route`) and understand the retirement of legacy `net-tools`.
- Inspect TCP/UDP sockets, listen states, and connection queues using `ss`.
- Dissect Linux DNS resolution paths (`/etc/resolv.conf`, `nsswitch.conf`, `systemd-resolved`, `dig`, `resolvectl`).
- Capture and analyze raw network traffic with `tcpdump` and Wireshark.
- Trace routing hops, packet loss, and MTU bottlenecks using `mtr`, `traceroute`, and `ping`.
- Build container networking from scratch using Linux Network Namespaces (`ip netns`) and virtual ethernet (`veth`) pairs.
- Systematically debug production networking outages from Layer 2 to Layer 7.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Linux Network Stack & Device Model](./01-Linux-Network-Stack-and-Device-Model.md) | Kernel network architecture, interface types, MTU, and driver offloading | ✅ Complete |
| 02 | [Modern Network Configuration (iproute2)](./02-Modern-Network-Configuration-iproute2.md) | `ip link`, `ip addr`, `ip route`, CIDR subnetting, and gateway routing | ✅ Complete |
| 03 | [Socket & Port Inspection (ss & netstat)](./03-Socket-and-Port-Inspection-ss-and-netstat.md) | TCP/UDP socket lifecycle, `ss` options, buffer queues, and connection states | ✅ Complete |
| 04 | [DNS Resolution Architecture & systemd-resolved](./04-DNS-Resolution-Architecture-and-systemd-resolved.md) | Name resolution pipeline, stub resolvers, `dig +trace`, and search domains | ✅ Complete |
| 05 | [Packet Analysis & Diagnostics](./05-Packet-Analysis-and-Diagnostics-tcpdump-traceroute-ping.md) | Deep packet inspection with `tcpdump`, BPF syntax, PMTU discovery, and `mtr` | ✅ Complete |
| 06 | [Network Namespaces & Container Networking](./06-Network-Namespaces-and-Container-Networking.md) | `ip netns`, `veth` pairs, bridge routing, and Kubernetes CNI foundations | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | High `TIME_WAIT` socket leaks, MTU blackholes, split-horizon DNS bugs | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Layer-by-layer systematic diagnostic methodology and recovery commands | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical interview questions for Junior to Staff DevOps/SRE roles | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Build container networking manually, capture packets, and debug DNS | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with in-depth technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | High-density command matrix, diagnostic flags, and kernel parameters | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 24: Package Management](../24-Linux-Package-Management-APT-YUM-DNF-APK-and-Source/README.md) | [Linux Roadmap Index](../README.md) | [01 - Linux Network Stack](./01-Linux-Network-Stack-and-Device-Model.md) |
