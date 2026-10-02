# 10 - Web Security, WAF and Hardening

Web servers and reverse proxies sit directly on the hostile public internet, making them the primary target for cyberattacks, SQL injection, cross-site scripting (XSS), HTTP request smuggling, Slowloris denial-of-service, and credential stuffing. Platform Engineers and DevSecOps practitioners must implement deep defense-in-depth, integrate Web Application Firewalls (ModSecurity / Coraza), enforce strict security headers, and harden reverse proxy pipelines against advanced protocol exploits.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [WAF Architecture & Inspection Phases](./01-Web-Application-Firewall-WAF-Architecture.md) | Reverse proxy WAF vs Cloud WAF, inspection phases (Request/Response headers, body). |
| 02 | [ModSecurity & Coraza with OWASP CRS](./02-ModSecurity-and-Coraza-with-OWASP-CRS.md) | ModSecurity v3, Coraza Go engine, OWASP Core Rule Set (CRS) v4, anomaly scoring mode. |
| 03 | [HTTP Security Headers: CSP, HSTS & Permissions](./03-HTTP-Security-Headers-CSP-HSTS-Permissions-Policy.md) | Content Security Policy (CSP), HSTS preload, X-Content-Type-Options, Permissions-Policy. |
| 04 | [Mitigating HTTP Request Smuggling & Splitting](./04-Mitigating-HTTP-Request-Smuggling-and-Splitting.md) | CL.TE and TE.CL vulnerabilities, RFC 7230 normalization, HTTP/2 downgrading exploits. |
| 05 | [DDoS Mitigation: Slowloris & Flood Protection](./05-DDoS-Mitigation-Slowloris-and-Flood-Protection.md) | Mitigating Slowloris, Slow POST attacks, SYN cookies, aggressive timeout tuning. |
| 06 | [Zero-Trust Edge: mTLS & API Gateway Hardening](./06-Zero-Trust-Edge-mTLS-and-API-Gateway-Hardening.md) | Edge client cert verification, header stripping, path traversal sanitization (`../`). |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: HTTP Request Smuggling auth bypass, CRS false positive checkout drop. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Analyzing ModSecurity audit logs (`modsec_audit.log`), whitelisting rule IDs, paranoia levels. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior DevSecOps/SRE questions on WAF anomaly scoring, smuggling attacks, and CSP. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: ModSecurity + OWASP CRS in Nginx; Lab 2: Security header audit; Lab 3: Slowloris defense. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for security headers, OWASP CRS tuning, and anti-smuggling rules. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Caddy & Modern HTTP/3](../09-Caddy-and-Modern-HTTP3-Web-Servers/README.md) | [README](./README.md) | [01 - WAF Architecture](./01-Web-Application-Firewall-WAF-Architecture.md) |
