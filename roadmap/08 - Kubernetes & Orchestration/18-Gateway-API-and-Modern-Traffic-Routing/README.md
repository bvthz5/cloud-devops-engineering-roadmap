# 18 - Gateway API and Modern Traffic Routing

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Gateway-API-vs-Ingress-The-Evolution-of-L7-Routing.md` — Ingress limitations, vendor annotation lock-in, and the architectural principles of Kubernetes Gateway API.
2. `02-Gateway-API-Role-Oriented-Architecture-GatewayClass-Gateway-Route.md` — Role-oriented design: Infrastructure Provider (`GatewayClass`), Cluster Operator (`Gateway`), and Developer (`HTTPRoute`).
3. `03-HTTPRoute-Advanced-Header-Matching-and-Path-Rewrites.md` — Native routing expressiveness: HTTP header/method/query matching, URL rewrites, and path redirects without annotations.
4. `04-Canary-Traffic-Splitting-and-Mirroring-with-HTTPRoute.md` — Advanced traffic management: percentage-based canary weighting and live request shadowing (mirroring).
5. `05-Cross-Namespace-Routing-and-ReferenceGrants.md` — Secure cross-namespace routing: `ReferenceGrant` handshake protocol, shared gateways, and multi-tenant isolation.
6. `06-Gateway-API-Implementations-Envoy-Gateway-Cilium-Traefik.md` — Production controllers: Envoy Gateway, Cilium Gateway API, Traefik, and Istio Ingress Gateway.
7. `07-Real-World-Scenarios.md` — Production post-mortems: cross-namespace security bypass prevented by ReferenceGrant, and traffic mirror capacity outage.
8. `08-Troubleshooting.md` — Diagnostic runbook for `Programmed: False` status conditions in Gateways and HTTPRoutes.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes Gateway API.
10. `10-Hands-On-Practice.md` — Production lab: deploying Gateway API with Canary traffic splitting and path rewriting.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density Gateway API reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 17 - Advanced Scheduling](../17-Advanced-Scheduling-Taints-Tolerations-and-Affinity/README.md) | [README](./README.md) | [01 - Gateway API vs Ingress](./01-Gateway-API-vs-Ingress-The-Evolution-of-L7-Routing.md) |
