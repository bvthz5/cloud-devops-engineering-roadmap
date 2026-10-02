# 03 — Endianness, Byte Order, and Memory Alignment

When a multi-byte data type (like a 32-bit integer or 64-bit memory address) is stored in physical memory or sent across a network cable, the order of its bytes matters.

---

## 1. Big-Endian vs Little-Endian

Consider the 32-bit hexadecimal value `0x12345678`:
- Most Significant Byte (MSB): `0x12`
- Least Significant Byte (LSB): `0x78`

```text
Memory Address:       0x00     0x01     0x02     0x03
------------------------------------------------------
Big-Endian:           0x12     0x34     0x56     0x78  (MSB stored first at lowest address)
Little-Endian:        0x78     0x56     0x34     0x12  (LSB stored first at lowest address)
```

- **Little-Endian:** Used by **x86, x86-64, AMD64, and most ARM64 chips (Linux/Android)**.
- **Big-Endian (Network Byte Order):** The standard for all internet protocols (TCP/IP, IPv4, IPv6 headers).

> [!IMPORTANT]
> When a Linux host sends an IP packet, the C kernel must convert host byte order to network byte order using helper functions like `htonl()` (Host to Network Long) and `ntohl()` (Network to Host Long).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Binary Arithmetic Twos Complement and Bitwise Operations](./02-Binary-Arithmetic-Twos-Complement-and-Bitwise-Operations.md) | [Index](../../../README.md) | [04 - Character Encoding ASCII Unicode and UTF8 →](./04-Character-Encoding-ASCII-Unicode-and-UTF8.md) |
