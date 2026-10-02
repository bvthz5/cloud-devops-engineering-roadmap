# 01 — OSI 7-Layer Reference Model

Created by the International Organization for Standardization (ISO) in 1984, the **Open Systems Interconnection (OSI)** model is a conceptual framework that standardizes network communication functions regardless of underlying hardware or technology.

---

## 1. The 7 Layers and Their Functions

```text
Layer 7: Application    ──► End-user protocols (HTTP, SSH, DNS, gRPC)
Layer 6: Presentation   ──► Data translation, encryption, compression (TLS, ASCII, JPEG)
Layer 5: Session        ──► Session establishment, checkpoints, RPC (SOCKS, NetBIOS)
Layer 4: Transport      ──► End-to-end connections, reliability, ports (TCP, UDP)
Layer 3: Network        ──► Logical addressing, routing, path finding (IPv4, IPv6, ICMP)
Layer 2: Data Link      ──► Physical addressing, framing, error check (Ethernet, MAC, ARP)
Layer 1: Physical       ──► Raw bit transmission over physical media (Cables, Fiber, Radio)
```

---

## 2. Protocol Data Units (PDUs)

At each layer of the OSI model, data has a specific technical name:

| Layer Number | Layer Name | Protocol Data Unit (PDU) | Key Addressing Scheme | Hardware / Device |
| :---: | :--- | :--- | :--- | :--- |
| **7** | Application | Data | Service identifiers, URLs | Web browsers, API clients |
| **6** | Presentation | Data | Encoding formats (UTF-8, ASN.1) | Cryptographic libraries |
| **5** | Session | Data | Session IDs, sockets | Operating system APIs |
| **4** | Transport | **Segment** (TCP) / **Datagram** (UDP) | Port numbers (e.g. 80, 443, 53) | L4 Load Balancers (NLB) |
| **3** | Network | **Packet** | IP addresses (e.g. 192.168.1.1) | Routers, L3 Switches |
| **2** | Data Link | **Frame** | MAC addresses (e.g. 52:54:00:12:34:56)| Network Switches, NICs |
| **1** | Physical | **Bits** (0s and 1s) | Voltage levels, light pulses | Cables, Hubs, Transceivers |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - TCP/IP 4-Layer Model](./02-TCPIP-4-Layer-Model-and-Comparison.md) |
