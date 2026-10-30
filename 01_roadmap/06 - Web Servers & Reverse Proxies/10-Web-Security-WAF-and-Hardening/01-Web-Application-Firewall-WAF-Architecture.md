# 01 - Web Application Firewall (WAF) Architecture

## 1. Network Firewall vs Web Application Firewall

```text
Layer 3/4 Network Firewall (e.g. iptables, AWS Security Groups):
- Inspects: Source/Destination IP, Port, Protocol (TCP/UDP).
- Blind to: HTTP payloads, SQL injection, XSS, malicious JSON.

Layer 7 Web Application Firewall (ModSecurity, Coraza, AWS WAF):
- Deeply inspects: HTTP headers, Cookies, POST bodies, JSON/XML payloads.
- Defends against: OWASP Top 10, SQLi, Cross-Site Scripting, Remote Code Execution (RCE).
```

---

## 2. The 5 WAF Processing Phases

A reverse proxy WAF evaluates requests across five distinct phases:

```text
Phase 1: Request Headers  ──► Inspects URI, Host, User-Agent, Cookie (Fast reject)
Phase 2: Request Body     ──► Inspects POST payload, multipart forms, JSON bodies
Phase 3: Response Headers ──► Inspects upstream headers, Content-Type, Set-Cookie
Phase 4: Response Body    ──► Scans outgoing HTML for leaked credit cards, API keys, stack traces
Phase 5: Logging          ──► Formats structured JSON audit log entry
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (09-Caddy-and-Modern-HTTP3-Web-Servers)](../09-Caddy-and-Modern-HTTP3-Web-Servers/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - ModSecurity and Coraza with OWASP CRS →](./02-ModSecurity-and-Coraza-with-OWASP-CRS.md) |
