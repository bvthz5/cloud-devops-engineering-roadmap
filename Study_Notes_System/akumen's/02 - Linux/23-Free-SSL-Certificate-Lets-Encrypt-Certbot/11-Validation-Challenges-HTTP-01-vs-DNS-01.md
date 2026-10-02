# 11 — Validation Challenges: HTTP-01 vs DNS-01

## 1. Challenge Comparison Matrix

| Challenge Type | Validation Mechanism | Requirements | Supports Wildcards? | Best Use Case |
|---|---|---|---|---|
| **HTTP-01** | Places token file at `http://domain/.well-known/acme-challenge/token` | Port 80 open to public internet. | **No** | Standard single domain or multi-domain web servers. |
| **DNS-01** | Places TXT record `_acme-challenge.domain.org` in DNS zone. | DNS API token / credentials. | **Yes** (`*.domain.com`) | Internal servers, wildcard certs, firewalled hosts. |

## 2. HTTP-01 Mechanics
1. Certbot contacts Let's Encrypt CA requesting certificate for `example.com`.
2. CA returns a challenge token string.
3. Certbot creates file `/var/www/html/.well-known/acme-challenge/<token>`.
4. Let's Encrypt server fetches `http://example.com/.well-known/acme-challenge/<token>`.
5. Upon successful HTTP 200 match, certificate is issued.

## 3. DNS-01 Mechanics
1. Certbot requests wildcard certificate for `*.example.com`.
2. CA returns a challenge string value.
3. Certbot creates DNS TXT record `_acme-challenge.example.com` with that string value.
4. Let's Encrypt queries public DNS for `_acme-challenge.example.com` TXT record.
5. Upon match, certificate is issued.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Automated Renewal](./10-Automated-Renewal-Certbot-Timer-and-Cron.md) | [README](./README.md) | [12 - Wildcard & Multi-Domain SAN Certs](./12-Wildcard-and-Multi-Domain-SAN-Certificates.md) |
