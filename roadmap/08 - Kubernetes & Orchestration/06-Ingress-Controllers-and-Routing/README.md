# 06 - Ingress Controllers and Routing

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Ingress-Resource-Specification-and-Path-Routing.md` — Layer 7 routing: host-based and path-based routing rules, and `pathType` (`Prefix`, `Exact`, `ImplementationSpecific`).
2. `02-Ingress-Nginx-Architecture-and-Controller-Mechanics.md` — Ingress-Nginx internals: controller loop, template rendering, Lua dynamic routing, and upstream syncing.
3. `03-SSL-TLS-Termination-and-Cert-Manager-Integration.md` — Automated PKI: cert-manager, `ClusterIssuer`, ACME HTTP-01 & DNS-01 challenges, and automated secret rotation.
4. `04-Traffic-Splitting-and-Canary-Ingress-Annotations.md` — Canary deployments via annotations: `canary-weight`, `canary-by-header`, and `canary-by-cookie`.
5. `05-Rewrite-Target-Custom-Headers-and-CORS-Policies.md` — URL transformation: `rewrite-target`, regex captures, CORS headers, and proxy buffer tuning.
6. `06-Ingress-Security-Rate-Limiting-and-ModSecurity-WAF.md` — Edge defense: client rate limiting, ModSecurity OWASP Core Rule Set (CRS), and SSL cipher hardening.
7. `07-Real-World-Scenarios.md` — Production post-mortems: the Nginx reload storm outage, and wildcard TLS renewal failures.
8. `08-Troubleshooting.md` — Diagnostic runbook for 404 Not Found, 502 Bad Gateway, and SSL handshake errors in Ingress.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Ingress controllers.
10. `10-Hands-On-Practice.md` — Production lab: deploying Ingress-Nginx with cert-manager automated TLS and path-based routing.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible answers.
12. `12-Quick-Revision.md` — High-density Ingress annotation and configuration cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 05 - Services](../05-Services-and-Service-Discovery/README.md) | [README](./README.md) | [01 - Ingress Specification](./01-Ingress-Resource-Specification-and-Path-Routing.md) |
