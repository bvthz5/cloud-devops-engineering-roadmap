# 01 — TCP Architecture and Reliability Guarantees

Transmission Control Protocol (TCP - RFC 793) provides a reliable, ordered, and error-checked stream of octets between applications running on hosts communicating via an IP network.

---

## 1. TCP Header Anatomy (20 Bytes Minimal)

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |        Urgent Pointer         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

## 2. Core Reliability Mechanisms

1. **Sequence Numbers & ACKs:** Every byte sent has a sequence number. The receiver acknowledges bytes received. Missing packets trigger retransmission after a Retransmission Timeout (RTO).
2. **Flow Control (Sliding Window):** The receiver advertises its available buffer space (`Window Size`). Prevents a fast sender from overwhelming a slow receiver.
3. **Congestion Control:** Prevents senders from saturating the network infrastructure:
   - **Cubic:** Traditional loss-based algorithm (assumes packet drop = congestion).
   - **BBR (Bottleneck Bandwidth and RTT):** Modern model-based algorithm developed by Google. Maximizes throughput while minimizing bufferbloat and latency.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-HTTP-HTTPS-and-Web-Protocols)](../04-HTTP-HTTPS-and-Web-Protocols/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - The TCP 3 Way Handshake and Teardown →](./02-The-TCP-3-Way-Handshake-and-Teardown.md) |
