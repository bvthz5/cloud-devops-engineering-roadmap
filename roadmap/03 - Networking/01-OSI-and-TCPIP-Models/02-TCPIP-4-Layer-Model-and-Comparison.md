# 02 — TCP/IP 4-Layer Model and Comparison

While the OSI model is an invaluable conceptual reference, the **TCP/IP Model** (created by DARPA) is the pragmatic architecture that actually powers the global Internet.

---

## 1. OSI vs TCP/IP Mapping

```text
OSI 7-LAYER MODEL                       TCP/IP 4-LAYER MODEL
+-----------------------+              +-----------------------+
| Layer 7: Application  |              |                       |
| Layer 6: Presentation | ───────────► | Application Layer     |
| Layer 5: Session      |              | (HTTP, DNS, SSH, TLS) |
+-----------------------+              +-----------------------+
| Layer 4: Transport    | ───────────► | Transport Layer       |
|                       |              | (TCP, UDP)            |
+-----------------------+              +-----------------------+
| Layer 3: Network      | ───────────► | Internet Layer        |
|                       |              | (IPv4, IPv6, ICMP)    |
+-----------------------+              +-----------------------+
| Layer 2: Data Link    | ───────────► | Network Access /      |
| Layer 1: Physical     |              | Link Layer (Ethernet) |
+-----------------------+              +-----------------------+
```

---

## 2. Why Did TCP/IP Win Over OSI?

1. **Working Code First:** TCP/IP was implemented in Berkeley Unix (BSD) and proved stable before international committees finalized OSI documentation.
2. **Pragmatic Simplicity:** Combining Session, Presentation, and Application into a single layer simplified developer APIs (the BSD Socket API).
3. **Open Standards:** The Request for Comments (RFC) process allowed rapid community-driven iteration, unlike ISO's proprietary standards.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - OSI 7-Layer Reference Model](./01-OSI-7-Layer-Reference-Model.md) | [README](./README.md) | [03 - Encapsulation & Decapsulation](./03-Encapsulation-and-Decapsulation-Data-Flow.md) |
