# 02 - Certificate Authorities, Chains, and Trust Stores

## 1. Public Key Infrastructure (PKI) Certificate Chain

A certificate establishes an unbroken chain of cryptographic trust from a trusted Root Certificate Authority down to your domain's leaf certificate.

```text
[ Root CA Certificate (Self-Signed) ]
  - Stored permanently in OS / Browser Trust Store (e.g., DigiCert Global Root G2)
  - Kept offline in an air-gapped safe
       │
       ▼ (Signs Intermediate Certificate)
[ Intermediate CA Certificate ]
  - Used for daily certificate issuance (e.g., Let's Encrypt R3)
       │
       ▼ (Signs Server Leaf Certificate)
[ Leaf / Server Certificate (*.example.com) ]
  - Installed on Nginx / HAProxy / Ingress Controller
  - Valid for 90 days to 398 days
```

---

## 2. The Missing Intermediate Certificate Pitfall

When deploying certificates in Nginx, `ssl_certificate` **must contain the full certificate bundle (fullchain)**:

```bash
# Correct bundle structure:
cat leaf_certificate.crt intermediate.crt > fullchain.pem
```

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    # CORRECT: Contains leaf + intermediate certificates
    ssl_certificate /etc/nginx/ssl/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/privkey.pem;
}
```

If only the leaf certificate is served:
- Desktop Chrome may silently download the missing intermediate via AIA (Authority Information Access).
- Android devices, `curl`, and automated API clients (Python `requests`, Go) will reject the connection with:
  `SSL: CERTIFICATE_VERIFY_FAILED: unable to get local issuer certificate`!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - TLS Handshake Architecture](./01-TLS-Handshake-Architecture-1.2-vs-1.3.md) | [README](./README.md) | [03 - Automated Certificates with ACME & Certbot](./03-Automated-Certificates-with-ACME-and-Certbot.md) |
