# 05 - Docker Networking

Docker networking enables isolated containers to communicate with each other, with the host machine, and with external internet services. This module covers the Container Network Model (CNM), the 5 built-in network drivers (bridge, host, none, overlay, macvlan), embedded DNS service discovery, and Linux kernel `iptables` packet filtering.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Docker Network Architecture & Drivers](./01-Docker-Network-Architecture-and-Drivers.md) | Container Network Model (CNM), driver comparison: bridge, host, none, overlay, macvlan. |
| 02 | [Bridge Networking & Virtual Ethernet Pairs](./02-Bridge-Networking-and-Veth-Pairs.md) | `docker0` vs user-defined bridges, `veth` pairs, routing packets through Linux bridge. |
| 03 | [Embedded DNS & Service Discovery](./03-Embedded-DNS-and-Service-Discovery.md) | 127.0.0.11 embedded DNS resolver, automatic container name resolution, network aliases. |
| 04 | [Port Publishing vs Exposing & iptables](./04-Port-Publishing-vs-Exposing-and-iptables.md) | `EXPOSE` vs `-p`, NAT packet traversal, `DOCKER` vs `DOCKER-USER` chain security rules. |
| 05 | [Host, None & Macvlan Networking](./05-Host-None-and-Macvlan-Networking.md) | High-performance `--network host`, air-gapped `--network none`, real LAN IPs via `macvlan`. |
| 06 | [Overlay Networks & Multi-Host VXLAN](./06-Overlay-Networks-and-Multi-Host-VXLAN.md) | Multi-host container networking, VXLAN encapsulation (UDP 4789), encrypted data plane. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Docker iptables bypass of UFW firewall, bridge subnet IP collision. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Inspecting networks with `docker network inspect`, sniffing veth traffic with `tcpdump`. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on bridge routing, embedded DNS, and firewall bypass. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Multi-network isolated communication; Lab 2: Securing Docker with DOCKER-USER; Lab 3: Macvlan. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for network commands, driver parameters, and iptables rules. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Docker Storage & Volumes](../04-Docker-Storage-and-Volumes/README.md) | [README](./README.md) | [01 - Docker Network Architecture](./01-Docker-Network-Architecture-and-Drivers.md) |
