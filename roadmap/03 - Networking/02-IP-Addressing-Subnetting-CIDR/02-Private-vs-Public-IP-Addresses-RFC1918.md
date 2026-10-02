# 02 — Private vs Public IP Addresses (RFC 1918)

Public IP addresses are globally routable across the public internet. Because IPv4 addresses are scarce, **RFC 1918** reserved three private address blocks for internal networks.

---

## 1. RFC 1918 Private Address Blocks

| Network Block | CIDR Prefix | Address Range | Total IP Addresses | Common Cloud Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **10.0.0.0** | `/8` | `10.0.0.0` – `10.255.255.255` | 16,777,216 | Large enterprise VPCs, Kubernetes Pod CIDRs |
| **172.16.0.0**| `/12` | `172.16.0.0` – `172.31.255.255` | 1,048,576 | Default Docker bridge network (`172.17.0.0/16`) |
| **192.168.0.0**| `/16` | `192.168.0.0` – `192.168.255.255` | 65,536 | Home routers, small office LANs |

---

## 2. Special Reserved IP Blocks

- **Loopback (`127.0.0.0/8`):** `127.0.0.1` (`localhost`). Inter-process communication on the local machine without touching hardware NICs.
- **Carrier-Grade NAT / CGNAT (`100.64.0.0/10`):** Used by ISPs and Kubernetes CNI overlays (e.g. Tailscale / AWS VPC secondary CIDRs).
- **Link-Local / APIPA (`169.254.0.0/16`):** Auto-assigned when DHCP fails.
  - **Crucial Cloud Identity Endpoint:** In AWS, GCP, and Azure, `http://169.254.169.254/` is the **Instance Metadata Service (IMDS)**, used by VMs to fetch IAM role credentials!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - IPv4 Addressing](./01-IPv4-Addressing-and-Classful-Architecture.md) | [README](./README.md) | [03 - Subnetting Fundamentals](./03-Subnetting-Fundamentals-and-Netmasks.md) |
