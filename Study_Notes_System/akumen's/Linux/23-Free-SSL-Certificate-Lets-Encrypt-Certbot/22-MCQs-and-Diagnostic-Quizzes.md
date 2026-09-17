# 22 — MCQs and Diagnostic Quizzes

## Question 1
Which port MUST be publicly accessible for Let's Encrypt HTTP-01 challenge verification?

- A) Port 443
- B) Port 80
- C) Port 8080
- D) Port 22

**Answer**: **B**
*Explanation*: HTTP-01 challenge requests tokens over standard unencrypted HTTP on port 80.

---

## Question 2
Which file should be referenced by Nginx directive `ssl_certificate`?

- A) `cert.pem`
- B) `chain.pem`
- C) `fullchain.pem`
- D) `privkey.pem`

**Answer**: **C**
*Explanation*: `fullchain.pem` includes both server certificate and intermediate CA chain, preventing untrusted root authority errors in client browsers.

---

## Question 3
Which Certbot challenge type is MANDATORY to issue wildcard certificates (`*.domain.com`)?

- A) HTTP-01
- B) TLS-ALPN-01
- C) DNS-01
- D) FTP-01

**Answer**: **C**
*Explanation*: Wildcard certificates require DNS-01 TXT record validation.

---

## Question 4
What command tests automated renewal without modifying live certificates?

- A) `certbot renew --force-renewal`
- B) `certbot renew --dry-run`
- C) `certbot test`
- D) `certbot status`

**Answer**: **B**
*Explanation*: `--dry-run` simulates the renewal workflow using Let's Encrypt staging servers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [21 - Interview Q&A](./21-DevOps-SSL-Interview-Questions-and-Answers.md) | [README](./README.md) | [23 - Quick Revision Notes](./23-Quick-Revision-Notes.md) |
