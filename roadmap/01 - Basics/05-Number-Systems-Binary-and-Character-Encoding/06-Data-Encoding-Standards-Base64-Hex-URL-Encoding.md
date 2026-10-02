# 06 — Data Encoding Standards: Base64, Hex, and URL Encoding

Encoding is the process of converting data from one format into another so it can be safely transmitted across systems designed for plaintext. **Encoding is NOT encryption** (it provides zero confidentiality).

---

## 1. Base64 Encoding (RFC 4648)

Base64 takes binary data and encodes it into 64 ASCII characters: `A-Z`, `a-z`, `0-9`, `+`, and `/`.
- It takes **3 bytes (24 bits)** of binary input and splits them into **4 chunks of 6 bits**.
- Each 6-bit chunk maps to a Base64 character.
- **Size Overhead:** Base64 increases data size by **~33%** (4 output bytes for every 3 input bytes).

```text
Input Bytes:       [ Byte 1 (8 bits) ] [ Byte 2 (8 bits) ] [ Byte 3 (8 bits) ]  (Total: 24 bits)
Split to 6 bits:   [ 6 bits ]      [ 6 bits ]      [ 6 bits ]      [ 6 bits ]   (Total: 4 characters)
```

### Padding (`=`):
If input data length is not divisible by 3, `=` padding characters are appended to reach a multiple of 4 characters.

---

## 2. Base64 in Kubernetes Secrets

Kubernetes Secrets are stored in `etcd` encoded in Base64 (not encrypted by default!):

```bash
# Encode a database password safely without a trailing newline
echo -n "SuperSecretPassword123!" | base64
# Output: U3VwZXJTZWNyZXRQYXNzd29yZDEyMyE=

# Decode a Kubernetes secret value
echo "U3VwZXJTZWNyZXRQYXNzd29yZDEyMyE=" | base64 -d
# Output: SuperSecretPassword123!
```

> [!WARNING]
> Always use `echo -n`! If you omit `-n`, `echo` appends an invisible `
` newline byte, causing your database password to include `
`, leading to authentication failures!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Line Endings CRLF vs LF](./05-Line-Endings-CRLF-vs-LF-and-Windows-Linux-Interop.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
