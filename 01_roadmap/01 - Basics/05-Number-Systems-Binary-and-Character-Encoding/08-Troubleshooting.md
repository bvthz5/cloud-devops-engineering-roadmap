# 08 — Troubleshooting Guide: Binary & Encoding

---

## 1. Quick Diagnostic Runbook

```bash
# 1. Detect file encoding and line endings
file filename.txt

# 2. View non-printable control characters
cat -v filename.txt

# 3. View hex representation of the first 32 bytes
hexdump -C -n 32 filename.txt

# 4. Strip Windows CRLF line endings
sed -i 's/$//' filename.sh

# 5. Remove UTF-8 BOM if present
sed -i '1s/^ï»¿//' filename.txt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
