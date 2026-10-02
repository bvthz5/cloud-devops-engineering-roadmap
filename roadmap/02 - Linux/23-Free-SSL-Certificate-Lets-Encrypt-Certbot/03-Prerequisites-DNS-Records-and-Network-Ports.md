# 03 — Prerequisites, DNS Records, and Network Ports

## 1. Prerequisites Checklist
Before requesting a Let's Encrypt SSL certificate, ensure:
1. You own or control a registered domain name (e.g. `example.com`).
2. Public IP address assigned to your server.
3. DNS **A record** (IPv4) or **AAAA record** (IPv6) configured.
4. TCP ports **80 (HTTP)** and **443 (HTTPS)** open on your firewall.

## 2. DNS Record Configuration

| Record Type | Name / Host | Value / Target | Purpose |
|---|---|---|---|
| **A** | `example.com` | `203.0.113.50` | Points root domain to web server IPv4 |
| **A** | `www` | `203.0.113.50` | Points `www` subdomain to web server IPv4 |
| **AAAA** | `@` | `2001:db8::1` | Points domain to web server IPv6 (optional) |

## 3. Verifying DNS Propagation

```bash
# Query A record using dig
dig +short example.com A
dig +short www.example.com A

# Verify DNS resolution using host command
host example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Let's Encrypt Architecture](./02-Lets-Encrypt-and-ACME-Protocol-Architecture.md) | [README](./README.md) | [04 - Firewall Setup (UFW & Cloud)](./04-Firewall-Configuration-UFW-and-Cloud-Security-Groups.md) |
