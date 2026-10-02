# 01 — IPv4 Addressing and Classful Architecture

An Internet Protocol Version 4 (IPv4) address is a 32-bit numerical label assigned to every device connected to an IP network.

---

## 1. Dotted-Decimal Notation

A 32-bit binary number is divided into four 8-bit groups called **Octets**, separated by dots:

```text
Binary:   11000000 . 10101000 . 00000001 . 00110010
Decimal:    192    .    168   .     1    .     50
```

---

## 2. Historical Classful Addressing (Obsolete)

Before 1993, IPv4 addresses were categorized into rigid classes based on their first octet:

| Class | Leading Bits | First Octet Range | Default Mask | Purpose / Allocation |
| :--- | :--- | :--- | :--- | :--- |
| **Class A** | `0...` | `1` to `126` | `255.0.0.0` (/8) | Huge organizations (16.7M hosts/network) |
| **Class B** | `10..` | `128` to `191` | `255.255.0.0` (/16)| Large companies (65,534 hosts/network) |
| **Class C** | `110.` | `192` to `223` | `255.255.255.0` (/24)| Small networks (254 hosts/network) |
| **Class D** | `1110` | `224` to `239` | N/A | Multicast groups |
| **Class E** | `1111` | `240` to `255` | N/A | Reserved for experimental research |

> [!NOTE]
> Classful addressing caused catastrophic address waste (an organization needing 300 hosts had to take a Class B with 65,534 addresses). It was replaced in 1993 by **CIDR**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-OSI-and-TCPIP-Models)](../01-OSI-and-TCPIP-Models/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Private vs Public IP Addresses RFC1918 →](./02-Private-vs-Public-IP-Addresses-RFC1918.md) |
