# 11 - Modern Protocols: Self-Assessment MCQs

### Q1. What allows QUIC connections to survive mobile IP address changes without dropping?
- A) IPsec MOBIKE
- B) Dynamic DNS updating
- C) 64-bit Connection ID (CID)
- D) BGP Anycast
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>QUIC identifies connections by a unique Connection ID rather than the IP/Port 4-tuple.</details>

---

### Q2. Which header compression algorithm is utilized by HTTP/3?
- A) GZIP
- B) HPACK
- C) Brotli
- D) QPACK
<details><summary><b>View Answer</b></summary><b>Correct Answer: D</b><br>HTTP/3 uses QPACK to allow out-of-order stream header compression without Head-of-Line blocking.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
