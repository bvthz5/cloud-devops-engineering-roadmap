# 06 — IPv6 Architecture and Migration

IPv4 provided ~4.3 billion addresses, which have officially been exhausted. IPv6 provides **128-bit addresses**, providing $2^{128} pprox 3.4 	imes 10^{38}$ unique addresses (enough to assign billions of IPs to every grain of sand on Earth).

---

## 1. IPv6 Format and Notation

An IPv6 address consists of 8 groups of 4 hexadecimal digits separated by colons:
`2001:0db8:85a3:0000:0000:8a2e:0370:7334`

### Compression Rules:
1. **Omit Leading Zeros:** `0000` -> `0`, `0db8` -> `db8`.
2. **Double Colon (`::`):** A single contiguous sequence of zero groups can be replaced with `::` (can only be used once per address!).
   - Compressed: `2001:db8:85a3::8a2e:370:7334`
   - Loopback: `0000:...:0001` -> `::1`

---

## 2. IPv6 Address Scopes

| Scope | Prefix | Description |
| :--- | :--- | :--- |
| **Global Unicast (GUA)** | `2000::/3` | Publicly routable across the global internet. |
| **Link-Local** | `fe80::/10` | Automatically configured on local link; not routed across routers. |
| **Unique Local (ULA)** | `fc00::/7` | Equivalent to IPv4 RFC 1918 private addresses. |
| **Multicast** | `ff00::/8` | Broadcast replacement in IPv6. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Cloud VPC Subnet Design Best Practices](./05-Cloud-VPC-Subnet-Design-Best-Practices.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
