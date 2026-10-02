# 12 — Quick Revision Cheat Sheet: Binary & Encoding

---

## 1. Number Systems Summary

| System | Base | Characters | DevOps Usage |
| :--- | :---: | :--- | :--- |
| **Binary** | 2 | `0, 1` | Subnet masks, CPU bits |
| **Octal** | 8 | `0-7` | File permissions (`0755`) |
| **Hex** | 16 | `0-9, A-F` | Memory addresses, MACs, IPv6, Hashes |

## 2. Essential Commands

```bash
echo -n "string" | base64       # Safe Base64 encode
echo "string" | base64 -d       # Base64 decode
dos2unix script.sh              # Convert CRLF to LF
cat -v script.sh                # Detect hidden ^M
file textfile.txt               # Detect file encoding
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-Compilers-Linkers-and-Runtimes) →](../06-Compilers-Linkers-and-Runtimes/01-The-Build-Pipeline-Source-to-Machine-Code.md) |
