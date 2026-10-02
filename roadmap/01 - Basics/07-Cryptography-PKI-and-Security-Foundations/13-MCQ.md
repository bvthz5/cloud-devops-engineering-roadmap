# 13 — Multiple Choice Questions: Cryptography & PKI

---

### Q1. Why is ECB (Electronic Codebook) mode considered dangerous for AES encryption?
- [ ] A) It requires too much memory
- [ ] B) It does not use an Initialization Vector (IV), encrypting identical plaintext blocks into identical ciphertext blocks and leaking data patterns
- [ ] C) It is an asymmetric algorithm
- [ ] D) It has been deprecated by quantum computers

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
ECB produces identical output blocks for identical input blocks, preserving recognizable structural patterns in the ciphertext.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Hands-On Practice](./12-Hands-On-Practice.md) | [README](./README.md) | [14 - Quick Revision](./14-Quick-Revision.md) |
