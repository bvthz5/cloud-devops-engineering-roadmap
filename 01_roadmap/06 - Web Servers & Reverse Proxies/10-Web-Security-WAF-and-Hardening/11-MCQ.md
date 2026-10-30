# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which HTTP security header instructs modern browsers to never load the site over plaintext HTTP for a specified duration?
- [ ] A) `Content-Security-Policy`
- [x] B) `Strict-Transport-Security` (HSTS)
- [ ] C) `X-XSS-Protection`
- [ ] D) `Sec-Fetch-Mode`

<details>
<summary><b>Explanation</b></summary>
HTTP Strict Transport Security (HSTS) mandates that browsers only communicate with the domain using secure HTTPS connections.
</details>

---

### 2. Which attack vectors are prevented by setting `X-Frame-Options: DENY` or `frame-ancestors 'none'`?
- [ ] A) Cross-Site Scripting (XSS)
- [ ] B) SQL Injection
- [x] C) Clickjacking
- [ ] D) DNS Rebinding

<details>
<summary><b>Explanation</b></summary>
These headers disallow rendering the web page inside an invisible iframe on malicious domains, preventing Clickjacking attacks.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
