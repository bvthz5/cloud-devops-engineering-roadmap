# 11 - APIs & Webhooks: Self-Assessment MCQs

### Q1. Which HTTP method is strictly defined as replacing an entire resource representation?
- A) `POST`
- B) `PATCH`
- C) `PUT`
- D) `UPDATE`
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>`PUT` replaces the resource in full, whereas `PATCH` applies partial updates.</details>

---

### Q2. Why must cryptographic hash comparison use `hmac.compare_digest` instead of standard `==`?
- A) Standard `==` does not support byte strings
- B) `hmac.compare_digest` executes in constant time, preventing timing-attack vulnerabilities
- C) `==` fails on hashes longer than 32 characters
- D) `compare_digest` automatically decrypts the hash
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Constant-time comparison ensures string comparison time does not leak cryptographic key data.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
