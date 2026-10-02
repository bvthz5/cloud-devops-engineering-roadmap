# 21 — DevOps SSL Interview Questions and Answers

## Q1: How does the ACME protocol validate domain ownership in Let's Encrypt?
**Answer**:
ACME supports two primary challenge types:
1. **HTTP-01**: CA returns a token. Client places token file at `http://<domain>/.well-known/acme-challenge/<token>`. CA fetches file via HTTP on port 80 to verify domain control.
2. **DNS-01**: Client creates a DNS TXT record `_acme-challenge.<domain>` containing a challenge token. CA queries public DNS to verify ownership. Required for wildcard certificates.

---

## Q2: Why are Let's Encrypt certificates valid for only 90 days?
**Answer**:
1. **Security**: Limits damage from compromised private keys or misplaced certificates.
2. **Automation**: Encourages system administrators to automate renewal workflows, eliminating forgotten manual annual renewals.

---

## Q3: What is the difference between `cert.pem`, `chain.pem`, `fullchain.pem`, and `privkey.pem`?
**Answer**:
- `cert.pem`: Public certificate for domain only.
- `chain.pem`: Intermediate CA certificate chain.
- `fullchain.pem`: Combination of `cert.pem` and `chain.pem`. Used in web server configs (`ssl_certificate`).
- `privkey.pem`: Private key corresponding to certificate. Keep secret!

---

## Q4: How do you obtain a Wildcard certificate (`*.example.com`) using Certbot?
**Answer**:
Wildcard certificates require the **DNS-01 challenge**. Use Certbot with a DNS plugin (e.g. `certbot-dns-cloudflare` or `certbot-dns-route53`) and pass credentials to dynamically update TXT records during validation.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [20 - Scenario Drills](./20-Scenario-Based-Troubleshooting-Drills.md) | [README](./README.md) | [22 - Scenario MCQs](./22-MCQs-and-Diagnostic-Quizzes.md) |
