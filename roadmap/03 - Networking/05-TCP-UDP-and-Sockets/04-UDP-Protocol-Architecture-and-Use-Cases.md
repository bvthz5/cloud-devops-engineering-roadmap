# 04 — UDP Protocol Architecture and Use Cases

User Datagram Protocol (UDP - RFC 768) provides lightweight, connectionless datagram transmission with zero reliability overhead.

---

## 1. UDP Header Anatomy (Only 8 Bytes!)

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

## 2. Why Choose UDP?

- **No Handshake:** Immediate transmission (zero round-trip delay).
- **No Head-of-Line Blocking:** Dropped packets don't delay subsequent packets.
- **No Congestion Throttle:** Transmits at wire speed.
- **Ideal For:** DNS queries (port 53), DHCP (ports 67/68), NTP time sync (port 123), video streaming (WebRTC), and **QUIC / HTTP/3**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - TCP States & Queues](./03-TCP-Connection-States-and-Socket-Queues.md) | [README](./README.md) | [05 - Network Sockets & BSD API](./05-Network-Sockets-and-the-BSD-Socket-API.md) |
