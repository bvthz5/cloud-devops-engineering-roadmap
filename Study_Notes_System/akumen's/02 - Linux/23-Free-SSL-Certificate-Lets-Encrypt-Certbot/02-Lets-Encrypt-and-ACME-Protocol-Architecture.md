# 02 — Let's Encrypt and ACME Protocol Architecture

## 1. What is Let's Encrypt?
**Let's Encrypt** is a free, automated, and open Certificate Authority (CA) run by the Internet Security Research Group (ISRG). It issues domain-validated (DV) X.509 certificates valid for **90 days**.

## 2. How the ACME Protocol Works (RFC 8555)
The **ACME (Automated Certificate Management Environment)** protocol automates the process of domain verification and certificate issuance.

```
+------------------+                    +--------------------+
|  Certbot Client  |                    |  Let's Encrypt CA  |
| (On Web Server)  |                    |    (ACME Server)   |
+------------------+                    +--------------------+
         |                                         |
         | -------- 1. Request Certificate ------> |
         |                                         |
         | <------- 2. Issue Challenge ---------- |
         |          (HTTP-01 or DNS-01)            |
         |                                         |
         | [3. Fulfills Challenge (places file     |
         |  or updates DNS record)]                |
         |                                         |
         | -------- 4. Notify Challenge Ready ---> |
         |                                         |
         |                               [5. CA Verifies Challenge]
         |                                         |
         | <------- 6. Certificate Issued -------- |
         v                                         v
```

## 3. Why 90-Day Lifetime?
1. **Security**: Limits damage from compromised private keys or stolen certificates.
2. **Automation**: Forces system administrators to automate renewal workflows via Cron or systemd timers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - SSL/TLS Fundamentals](./01-SSL-TLS-and-HTTPS-Fundamentals.md) | [README](./README.md) | [03 - Prerequisites & DNS Records](./03-Prerequisites-DNS-Records-and-Network-Ports.md) |
