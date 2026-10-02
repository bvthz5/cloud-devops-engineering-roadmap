# 02 — Binary Arithmetic, Two's Complement, and Bitwise Operations

Bitwise operations allow computers to manipulate individual bits directly with single CPU clock cycles. In DevOps, bitwise operations govern IP subnet calculations (CIDR), network routing masks, and process privilege masks (`umask`).

---

## 1. Bitwise Operators

| Operator | Symbol | Description | Example (A = 1010, B = 1100) | Result |
| :--- | :---: | :--- | :--- | :---: |
| **AND** | `&` | 1 only if BOTH bits are 1 | `1010 & 1100` | `1000` |
| **OR** | `\|` | 1 if EITHER bit is 1 | `1010 \| 1100` | `1110` |
| **XOR** | `^` | 1 if bits are DIFFERENT | `1010 ^ 1100` | `0110` |
| **NOT** | `~` | Inverts all bits (0 -> 1, 1 -> 0)| `~1010` (8-bit) | `11110101` |
| **Left Shift** | `<<`| Shifts bits left (multiplies by 2) | `0001 << 2` | `0100` (4) |
| **Right Shift**| `>>`| Shifts bits right (divides by 2) | `1000 >> 2` | `0010` (2) |

---

## 2. CIDR Subnet Mask Calculation via Bitwise AND

How does a Linux router determine if `192.168.1.50` and `192.168.1.200` are on the same local network when configured with a `/24` (`255.255.255.0`) subnet mask?

```text
IP Address:   192.168.1.50   ->  11000000.10101000.00000001.00110010
Subnet Mask:  255.255.255.0  ->  11000000.11111111.11111111.00000000
-----------------------------------------------------------------------
Bitwise AND:                     11000000.10101000.00000001.00000000
Network ID:                      192.168.1.0
```
Because `(IP & Netmask) == Network ID`, the kernel immediately knows whether to route locally via ARP or forward to the default gateway.

---

## 3. Two's Complement (Signed Integers)

How do computers store negative numbers in binary?
Modern CPUs use **Two's Complement**:
1. Take the positive binary representation.
2. Invert all bits (bitwise NOT).
3. Add `1`.

Example for `-5` in an 8-bit integer:
```text
+5 in binary:      00000101
Invert bits (NOT): 11111010
Add 1:             11111011  ===> -5 in Two's Complement
```
The most significant bit (MSB) acts as the sign bit: `0` = positive, `1` = negative.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Number Systems](./01-Number-Systems-Decimal-Binary-Octal-Hexadecimal.md) | [README](./README.md) | [03 - Endianness & Byte Order](./03-Endianness-Byte-Order-and-Memory-Alignment.md) |
