# 01 — Number Systems: Decimal, Binary, Octal, and Hexadecimal

All digital computers operate exclusively on binary electrical states (high voltage / low voltage, representing 1 and 0). To make binary human-readable and computationally efficient, computer scientists use **Octal** (Base 8) and **Hexadecimal** (Base 16).

---

## 1. The Four Core Number Systems

| Number System | Base (Radix) | Allowed Digits | Why Computer Systems Use It |
| :--- | :---: | :--- | :--- |
| **Binary** | Base 2 | `0, 1` | Physical hardware transistor logic (on/off). |
| **Octal** | Base 8 | `0, 1, 2, 3, 4, 5, 6, 7` | Exactly represents **3 bits** ($2^3 = 8$). Used for Linux file permissions (`rwx`). |
| **Decimal** | Base 10 | `0, 1, 2, 3, 4, 5, 6, 7, 8, 9` | Standard human everyday arithmetic. |
| **Hexadecimal** | Base 16 | `0-9` and `A, B, C, D, E, F` | Exactly represents **4 bits (1 nibble)** ($2^4 = 16$). 2 hex digits = 1 byte. Used for memory addresses, MAC addresses, IPv6, and cryptographic hashes. |

---

## 2. Why Linux File Permissions Use Octal (Base 8)

In Linux, file permissions consist of three permission bits for three entity classes: **Owner (User)**, **Group**, and **Others**.

Each entity has 3 independent binary flags:
- Read (`r`) = 4 ($2^2$)
- Write (`w`) = 2 ($2^1$)
- Execute (`x`) = 1 ($2^0$)

```text
Binary Bits:   r  w  x  |  r  w  x  |  r  w  x
Value:         4  2  1  |  4  2  1  |  4  2  1
------------------------------------------------
Example 1:     1  1  1  |  1  0  1  |  1  0  1
Sum:           4+2+1=7  |  4+0+1=5  |  4+0+1=5  ===> chmod 755 (rwxr-xr-x)

Example 2:     1  1  0  |  1  0  0  |  1  0  0
Sum:           4+2+0=6  |  4+0+0=4  |  4+0+0=4  ===> chmod 644 (rw-r--r--)
```
Because each entity's permissions are represented by exactly **3 bits**, an octal digit (0 to 7) maps 1-to-1 without conversion loss!

---

## 3. Why Hexadecimal (Base 16) Dominates Systems

One byte equals 8 bits.
- Binary: `11011011` (8 characters, hard to read)
- Decimal: `219` (unclear which bits are set)
- Hexadecimal: `0xDB` (exactly 2 characters per byte: `D` = 1101, `B` = 1011)

Every byte in memory, network packets, and disk storage maps cleanly to two hexadecimal digits:
- IPv6: `2001:0db8:85a3:0000:0000:8a2e:0370:7334` (128 bits = 32 hex digits)
- MAC Address: `52:54:00:12:34:56` (48 bits = 6 hex pairs)
- SHA-256 Hash: 256 bits = 64 hexadecimal characters.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Data-Formats-YAML-JSON-XML-TOML)](../04-Data-Formats-YAML-JSON-XML-TOML/14-Quick-Revision.md) | [Index](../../../README.md) | [02 - Binary Arithmetic Twos Complement and Bitwise Operations →](./02-Binary-Arithmetic-Twos-Complement-and-Bitwise-Operations.md) |
