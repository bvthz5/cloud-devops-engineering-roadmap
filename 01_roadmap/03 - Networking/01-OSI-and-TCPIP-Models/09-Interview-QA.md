# 09 — OSI & TCP/IP Interview Q&A

10 technical interview questions for DevOps, SRE, and Systems roles.

---

### Q1: What is the difference between a Packet and a Frame?
**Answer:**
A **Frame** is the Protocol Data Unit (PDU) at Layer 2 (Data Link layer). It encapsulates payload data with source and destination **MAC addresses** and a trailing CRC checksum.
A **Packet** is the PDU at Layer 3 (Network layer). It encapsulates data with source and destination **IP addresses**, TTL, and routing metadata. An IP packet is placed inside the payload section of an Ethernet frame.

---

### Q2: Why is Layer 4 load balancing faster than Layer 7 load balancing?
**Answer:**
Layer 4 load balancers only inspect the IP and TCP/UDP header (the first ~40 bytes of a packet). They do not decrypt TLS or reconstruct HTTP requests, often passing packets directly at the network interface or kernel level (via IPVS/eBPF).
Layer 7 load balancers must terminate TLS, buffer and parse the complete HTTP request (headers, path, cookies), make complex routing decisions, and establish a separate TCP connection to the backend, consuming significantly more CPU and memory.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
