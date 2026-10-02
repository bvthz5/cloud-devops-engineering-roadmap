# Module 26: Linux Firewalls (iptables, nftables, UFW, firewalld)

Linux packet filtering and firewalling provide frontline security defense for bare-metal servers, virtual machines, cloud instances, and container clusters. Every incoming or outgoing network packet traverses the Linux kernel's Netfilter subsystem. Understanding how firewall rules, Network Address Translation (NAT), and connection tracking operate is crucial for securing cloud workloads and debugging container networking.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand the Linux kernel **Netfilter** packet traversal flow across all 5 hook points.
- Master legacy **iptables** tables (`filter`, `nat`, `mangle`, `raw`) and chains.
- Leverage modern **nftables** for high-performance, atomic packet filtering and sets.
- Secure Debian/Ubuntu hosts using **UFW** (Uncomplicated Firewall) with rate limiting.
- Manage enterprise RHEL/Rocky Linux firewalls using **firewalld** zones and rich rules.
- Implement Source NAT (SNAT), Destination NAT (DNAT), and Port Forwarding.
- Resolve critical production issues such as Docker bypassing host UFW firewalls.
- Troubleshoot dropped packets using `LOG` targets and Netfilter connection tracking (`conntrack`).

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Netfilter Architecture & Packet Traversal Flow](./01-Linux-Packet-Filtering-and-Netfilter-Architecture.md) | The 5 Netfilter hooks, packet life cycle, and kernel hooks | ✅ Complete |
| 02 | [iptables Deep Dive & Rule Management](./02-iptables-Deep-Dive-and-Rule-Management.md) | Tables, chains, targets (ACCEPT, DROP, REJECT), and stateful filtering | ✅ Complete |
| 03 | [nftables: Modern Linux Firewall](./03-nftables-The-Modern-Linux-Firewall.md) | Next-gen unified packet engine, atomic reloads, sets, and syntax | ✅ Complete |
| 04 | [UFW for Debian & Ubuntu](./04-UFW-Uncomplicated-Firewall-for-Debian-Ubuntu.md) | Default policies, port rules, CIDR restrictions, and rate limiting | ✅ Complete |
| 05 | [firewalld for RHEL & Rocky Linux](./05-firewalld-Dynamic-Firewall-for-RHEL-Rocky.md) | Dynamic zones, services, rich rules, runtime vs `--permanent` | ✅ Complete |
| 06 | [NAT, Port Forwarding & Masquerading](./06-NAT-Port-Forwarding-and-Masquerading.md) | SNAT, DNAT, container port publishing, and Docker iptables architecture | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Docker UFW bypass bug, conntrack table exhaustion, SYN flood mitigation | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Tracing dropped packets, rule evaluation order, and connection resets | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical interview questions for DevOps, SRE, and Cloud Security | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Configure UFW, build an iptables NAT router, and create nftables sets | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical rationales | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Command matrix comparing iptables, nftables, UFW, and firewalld | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 25: Networking & DNS](../25-Linux-Networking-and-DNS-Troubleshooting/README.md) | [Linux Roadmap Index](../README.md) | [01 - Netfilter Architecture](./01-Linux-Packet-Filtering-and-Netfilter-Architecture.md) |
