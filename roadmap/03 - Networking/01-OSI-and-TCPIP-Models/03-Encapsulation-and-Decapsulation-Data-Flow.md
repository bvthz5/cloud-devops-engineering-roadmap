# 03 — Encapsulation and Decapsulation Data Flow

Communication between two hosts across a network involves packing data with protocol metadata (headers) on transmission, and unpacking it on reception.

---

## 1. The Encapsulation Process (Sending Host)

When a web browser sends an HTTP request:

```text
[ Application Data ]                                      "GET /index.html HTTP/1.1"
         │
         ▼  (Layer 4: Adds TCP Header)
[ TCP Header (Src Port 52140, Dst Port 80, Seq#) ][ Data ]  ===> TCP Segment
         │
         ▼  (Layer 3: Adds IP Header)
[ IP Header (Src 192.168.1.5, Dst 93.184.216.34, TTL=64) ][ TCP Header ][ Data ]  ===> IP Packet
         │
         ▼  (Layer 2: Adds Ethernet Frame Header & Trailer)
[ Ethernet Header (Src MAC, Dst MAC, Type) ][ IP Header ][ TCP Header ][ Data ][ CRC Trailer ] ===> Ethernet Frame
         │
         ▼  (Layer 1: Transmitted as physical bits)
010110010101010101001010101010101101010101010101010101010101010101010101
```

---

## 2. The Decapsulation Process (Receiving Server)

1. **Physical Layer (NIC):** Converts electrical/optical signals into bytes.
2. **Data Link Layer:** Validates Ethernet CRC checksum. Checks destination MAC address. Strips Ethernet header.
3. **Network Layer:** Validates destination IP address. Checks TTL. Decrements TTL. Reassembles fragments if necessary. Strips IP header.
4. **Transport Layer:** Verifies TCP checksum. Checks sequence numbers and acknowledges packets. Delivers payload to the socket mapped to destination port `80`.
5. **Application Layer:** Web server process (e.g. NGINX) reads raw HTTP payload.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - TCP/IP 4-Layer Model](./02-TCPIP-4-Layer-Model-and-Comparison.md) | [README](./README.md) | [04 - Layer 2 Data Link](./04-Layer-2-Data-Link-MAC-and-Switches.md) |
