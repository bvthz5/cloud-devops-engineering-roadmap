# 03 - Cloud & DevOps Networking Engineering

> Comprehensive, production-grade guide to computer networking, cloud VPC topology, container SDN/CNI, edge acceleration, and modern web protocols for Cloud, DevOps, and Site Reliability Engineers.

---

## 🎯 Architecture & Roadmap Overview

Networking is the nervous system of modern cloud architectures, microservices, and distributed systems. This track progresses from foundational packet flows up to hyperscale BGP routing, eBPF container overlays, Anycast edge CDNs, and binary protocols.

---

## 📌 Module Directory

| Module # | Domain | Core Focus Areas | Status |
|:---:|:---|:---|:---:|
| **01** | [01 - OSI and TCP/IP Models](./01-OSI-and-TCPIP-Models/README.md) | 7-layer vs 4-layer comparison, PDU encapsulation, packet traversal lifecycle | ✅ Complete |
| **02** | [02 - IP Addressing, Subnetting & CIDR](./02-IP-Addressing-Subnetting-CIDR/README.md) | IPv4/IPv6, bitmasks, VLSM, RFC 1918 private spaces, cloud CIDR planning | ✅ Complete |
| **03** | [03 - DNS and DHCP](./03-DNS-and-DHCP/README.md) | Recursive resolution, record types (A, CNAME, ALIAS), CoreDNS in k8s, DORA | ✅ Complete |
| **04** | [04 - HTTP, HTTPS and Web Protocols](./04-HTTP-HTTPS-and-Web-Protocols/README.md) | HTTP/1.1 vs 2.0 vs 3.0, TLS 1.3 handshakes, caching headers, CORS, security | ✅ Complete |
| **05** | [05 - TCP, UDP and Sockets](./05-TCP-UDP-and-Sockets/README.md) | 3-way handshake, teardown, TIME_WAIT tuning, socket queues, BSD socket API | ✅ Complete |
| **06** | [06 - SSH and Secure Remote Access](./06-SSH-and-Secure-Remote-Access/README.md) | Ed25519 cryptography, sshd hardening, bastions, ProxyJump, dynamic tunnels | ✅ Complete |
| **07** | [07 - Firewalls, iptables, nftables & UFW](./07-Firewalls-iptables-and-UFW/README.md) | Netfilter hooks, iptables tables/chains, conntrack limits, Docker/K8s bypass | ✅ Complete |
| **08** | [08 - Load Balancers, Proxies & Traffic Control](./08-Load-Balancers-and-Proxies/README.md) | L4 vs L7, NGINX, Envoy, Ketama consistent hashing, health flap damping | ✅ Complete |
| **09** | [09 - VPN and Cloud VPC Networking](./09-VPN-and-VPC-Networking/README.md) | IPsec, WireGuard, Multi-AZ VPCs, TGW hub-and-spoke, PrivateLink, ZTNA | ✅ Complete |
| **10** | [10 - Network Troubleshooting Tools](./10-Network-Troubleshooting-Tools/README.md) | `tcpdump`, Wireshark, `mtr`, `ss`, `iperf3`, high-precision `curl -w` timings | ✅ Complete |
| **11** | [11 - BGP Routing & Cloud Interconnects](./11-BGP-Routing-and-Cloud-Interconnects/README.md) | Autonomous Systems, eBGP/iBGP, AWS Direct Connect, Calico/MetalLB BGP | ✅ Complete |
| **12** | [12 - Overlay Networks, VXLAN & Container CNI](./12-Overlay-Networks-VXLAN-and-Container-CNI/README.md) | VXLAN 24-bit VNI, Geneve, CNI spec, Flannel/Calico/Cilium/AWS VPC CNI, MTU | ✅ Complete |
| **13** | [13 - CDN, Edge Networks & Anycast Routing](./13-CDN-Edge-Networks-and-Anycast-Routing/README.md) | BGP Anycast, Origin Shields, `stale-while-revalidate`, Edge Compute, DDoS | ✅ Complete |
| **14** | [14 - Modern Web Protocols: HTTP/3, QUIC & gRPC](./14-Modern-Web-Protocols-HTTP3-QUIC-and-gRPC/README.md) | QUIC UDP transport, 0-RTT, Connection Migration, QPACK, gRPC Protobuf | ✅ Complete |

---

## 🛠️ Module Structure Standards

Every module contains 13 dedicated, production-ready files:
1. `README.md` — Curriculum syllabus, prerequisites, and learning objectives.
2. `01` to `06` — Deep architectural engineering concepts with diagrams and packet traces.
3. `07-Real-World-Scenarios.md` — Real production outages, SRE post-mortems, and incident analysis.
4. `08-Troubleshooting.md` — Diagnostic decision trees, metrics, and step-by-step triage runbooks.
5. `09-Interview-QA.md` — 10 high-frequency senior SRE and DevOps interview questions.
6. `10-Hands-On-Practice.md` — Real-world CLI labs with step-by-step terminal commands.
7. `11-MCQ.md` — 10 scenario-based self-assessment multiple-choice questions.
8. `12-Quick-Revision.md` — High-density cheat sheets and command reference tables.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Linux Foundations](../02%20-%20Linux/README.md) | [Master Roadmap Index](../00-Master-Index.md) | [01 - OSI & TCP/IP Models](./01-OSI-and-TCPIP-Models/README.md) |
