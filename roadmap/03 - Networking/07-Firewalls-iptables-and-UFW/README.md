# Module 07: Firewalls, iptables, nftables, and UFW

Welcome to **Module 07: Firewalls, iptables, nftables, and UFW**. Packet filtering, stateful firewall inspection, and network address translation (NAT) form the bedrock of Linux security, container networking, and cloud perimeter defense.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Master the Linux **Netfilter** kernel subsystem, hooks (`PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING`), and packet evaluation order.
2. Construct and audit **iptables** tables (`filter`, `nat`, `mangle`, `raw`, `security`) and chains.
3. Understand **conntrack** (connection tracking states: `NEW`, `ESTABLISHED`, `RELATED`, `INVALID`) and prevent state table exhaustion.
4. Transition between legacy **iptables**, modern **nftables**, **UFW** (Ubuntu), and **firewalld** (RHEL).
5. Understand the critical interactions between Docker/Kubernetes iptables rules (`DOCKER-USER`, `KUBE-SERVICES`) and host firewalls.
6. Troubleshoot dropped packets, silent packet filtering, and performance bottlenecks in high-throughput environments.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Linux Netfilter Architecture](./01-Linux-Netfilter-Architecture-and-Hooks.md) | The 5 kernel hooks, packet flow through tables and chains |
| 02 | [iptables Architecture & Rules](./02-iptables-Tables-Chains-and-Rule-Syntax.md) | Filter, NAT, Mangle tables; rule syntax, targets (ACCEPT, DROP, REJECT, MASQUERADE) |
| 03 | [Stateful Inspection & Conntrack](./03-Stateful-Firewall-Inspection-and-Conntrack.md) | Connection tracking states, conntrack table sizing, tuning `nf_conntrack_max` |
| 04 | [nftables Modern Packet Classification](./04-nftables-Modern-Linux-Packet-Classification.md) | Next-generation packet filtering, unified syntax, performance improvements |
| 05 | [Host Firewalls: UFW & firewalld](./05-Host-Firewalls-UFW-and-firewalld-Management.md) | Managing UFW on Ubuntu and firewalld on RHEL/CentOS, profiles, rich rules |
| 06 | [Docker & Kubernetes iptables Integration](./06-Docker-and-Kubernetes-iptables-Integration.md) | How Docker bypasses UFW, DOCKER-USER chain, kube-proxy iptables mode |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Conntrack table full outage, Docker publishing port to public internet, MTU/MSS clamping |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | `iptables -vnL`, `conntrack -L`, dropped packet counters, logging dropped packets |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Hardening a bastion host, writing DNAT port forwarding, configuring UFW securely |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Fast reference commands, tables cheat sheet, and rule templates |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - SSH & Remote Access](../06-SSH-and-Secure-Remote-Access/README.md) | [Networking Index](../README.md) | [01 - Netfilter Architecture](./01-Linux-Netfilter-Architecture-and-Hooks.md) |
