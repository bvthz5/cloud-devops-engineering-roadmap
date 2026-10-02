# 01 - Evolution from TCP to QUIC UDP Transport

## 1. The Two Fatal Bottlenecks of TCP

### 1. TCP Ossification
Over the past 40 years, billions of intermediate routers, firewalls, and NAT devices ("middleboxes") across the internet have hard-coded assumptions about TCP packet formats. If researchers attempt to add new TCP options or change headers, middleboxes drop the packets. Upgrading TCP across the global internet became virtually impossible.

### 2. Transport-Layer Head-of-Line (HoL) Blocking
HTTP/2 introduced application-level multiplexing (sending 100 requests over 1 single TCP connection).
**The flaw:** If a single packet belonging to Stream 5 is lost in transit, **the TCP stack pauses delivery of all other 99 streams** until that lost packet is retransmitted and acknowledged!

---

## 2. Why QUIC Runs on UDP

```
+-----------------------------------+          +-----------------------------------+
|              HTTP/2               |          |              HTTP/3               |
+-----------------------------------+          +-----------------------------------+
|             TLS 1.2/1.3           |          |               QUIC                |
+-----------------------------------+          | (Congestion, Flow Control, TLS 1.3|
|                TCP                |          |  Connection ID, Stream Multiplex) |
+-----------------------------------+          +-----------------------------------+
|                IP                 |          |                UDP                |
+-----------------------------------+          +-----------------------------------+
|             Physical              |          |                IP                 |
+-----------------------------------+          +-----------------------------------+
```

- **UDP is ubiquitous:** Every firewall and router forwards UDP without interference.
- **User-Space Innovation:** QUIC implements encryption, congestion control, and loss recovery in **user-space** on top of raw UDP datagrams. Application developers can deploy transport updates instantly without waiting for OS kernel patches!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (13-CDN-Edge-Networks-and-Anycast-Routing)](../13-CDN-Edge-Networks-and-Anycast-Routing/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - QUIC Protocol Internals 0 RTT and Connection Migration →](./02-QUIC-Protocol-Internals-0-RTT-and-Connection-Migration.md) |
