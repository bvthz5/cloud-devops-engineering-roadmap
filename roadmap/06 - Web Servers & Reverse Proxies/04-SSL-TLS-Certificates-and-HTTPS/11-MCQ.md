# 11 - Multiple-Choice Assessment (MCQ)

### 1. How many round trips (RTT) does a standard TLS 1.3 handshake require before client application data can be sent?
- [ ] A) 2-RTT
- [x] B) 1-RTT
- [ ] C) 0-RTT
- [ ] D) 3-RTT

<details>
<summary><b>Explanation</b></summary>
A fresh TLS 1.3 handshake requires exactly 1-RTT (one round trip). Subsequent resumptions can use 0-RTT early data.
</details>

---

### 2. In Nginx mTLS, which directive instructs the server to strictly require and validate a client certificate?
- [ ] A) `ssl_client_verify optional;`
- [ ] B) `ssl_require_mtls on;`
- [x] C) `ssl_verify_client on;`
- [ ] D) `ssl_authenticate_client required;`

<details>
<summary><b>Explanation</b></summary>
<code>ssl_verify_client on;</code> mandates that incoming requests must present a valid certificate signed by the CA defined in <code>ssl_client_certificate</code>.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
