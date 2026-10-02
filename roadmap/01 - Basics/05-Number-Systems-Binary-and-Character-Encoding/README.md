# Submodule 05: Number Systems, Binary, and Character Encoding

Foundational data representation principles are at the core of all computing. DevOps engineers frequently deal with IP subnet calculations, octal file permissions (`0755`), hexadecimal memory addresses and MACs, Base64-encoded Kubernetes secrets, and cross-platform line ending bugs (`CRLF` vs `LF`).

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Convert seamlessly between **Decimal, Binary, Octal, and Hexadecimal**.
- Understand why Linux file permissions use **Octal** and why IPv6/MAC addresses use **Hexadecimal**.
- Perform **Bitwise Operations** (AND, OR, XOR, NOT, Bit shifts) to calculate CIDR subnets and `umask` values.
- Explain **Endianness** (Big-Endian vs Little-Endian) and Network Byte Order.
- Dissect character encodings: **ASCII, Unicode code points, and UTF-8** variable-length encoding.
- Troubleshoot and fix cross-platform **Line Ending issues (CRLF vs LF, `^M` errors)** in Docker containers and bash scripts.
- Master data encodings: **Base64** mechanics, padding, Base64URL, and hex representations.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Number Systems: Decimal, Binary, Octal, Hex](./01-Number-Systems-Decimal-Binary-Octal-Hexadecimal.md) | Radix conversions, octal permissions (0755), and hexadecimal memory | ✅ Complete |
| 02 | [Binary Arithmetic & Bitwise Operations](./02-Binary-Arithmetic-Twos-Complement-and-Bitwise-Operations.md) | Two's complement, AND/OR/XOR/NOT, CIDR subnetting, and umask math | ✅ Complete |
| 03 | [Endianness & Memory Byte Order](./03-Endianness-Byte-Order-and-Memory-Alignment.md) | Big-Endian (Network Order) vs Little-Endian (x86), memory alignment | ✅ Complete |
| 04 | [Character Encoding: ASCII, Unicode & UTF-8](./04-Character-Encoding-ASCII-Unicode-and-UTF8.md) | 7-bit ASCII, Unicode code points, UTF-8 byte serialization, and BOM | ✅ Complete |
| 05 | [Line Endings: CRLF vs LF & Cross-Platform Gotchas](./05-Line-Endings-CRLF-vs-LF-and-Windows-Linux-Interop.md) | `
` vs `
`, the `^M` Docker error, `dos2unix`, and `.gitattributes` | ✅ Complete |
| 06 | [Data Encoding Standards: Base64 & Hex](./06-Data-Encoding-Standards-Base64-Hex-URL-Encoding.md) | 6-bit mapping, padding (`=`), Base64 in Kubernetes Secrets, and JWTs | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Container crash on `^M: bad interpreter`, secret corruption via base64 | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Detecting hidden bytes with `cat -v`, `file`, `hexdump`, and `sed` fixes | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical questions on binary, encoding, and data representation | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Bitwise subnet calculations, fixing CRLF in containers, safe Base64 | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Conversion tables, ASCII chart, bitwise truth tables, and commands | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Data Formats](../04-Data-Formats-YAML-JSON-XML-TOML/README.md) | [01 - Basics Index](../README.md) | [01 - Number Systems](./01-Number-Systems-Decimal-Binary-Octal-Hexadecimal.md) |
