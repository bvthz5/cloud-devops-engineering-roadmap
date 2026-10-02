# 12 - RBAC and Cluster Security

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Authentication-X509-OIDC-and-Tokens.md` — Identity in Kubernetes: X.509 client certificates, OIDC (Okta, Keycloak), webhook tokens, and anonymous auth risks.
2. `02-Role-ClusterRole-RoleBinding-and-ClusterRoleBinding.md` — Granular authorization: API groups, resources, verbs (`get`, `list`, `watch`, `create`, `delete`), and namespace vs cluster scope.
3. `03-ServiceAccounts-and-Bound-Token-Projection.md` — Machine identities: Bound ServiceAccount token projection (`TokenRequest` API, audience, TTL, rotation).
4. `04-Pod-Security-Admission-PSA-Privileged-Baseline-Restricted.md` — Replacing PSPs: Pod Security Standards (`privileged`, `baseline`, `restricted`) and enforcement modes (`enforce`, `audit`, `warn`).
5. `05-Admission-Controllers-Mutating-and-Validating-Webhooks.md` — Extensible cluster guardrails: dynamic admission webhooks, failure policies (`Fail` vs `Ignore`), and certificate bundling.
6. `06-Policy-as-Code-with-Kyverno-and-OPA-Gatekeeper.md` — Modern policy engines: Kyverno YAML policies vs OPA Gatekeeper Rego constraint templates.
7. `07-Real-World-Scenarios.md` — Production post-mortems: cluster-admin privilege escalation breach, and webhook failure locking out all cluster deployments.
8. `08-Troubleshooting.md` — Diagnostic runbook for `forbidden: User cannot list resource` and failing admission webhooks.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes security architecture.
10. `10-Hands-On-Practice.md` — Production lab: crafting least-privilege RBAC roles and testing permissions via `kubectl auth can-i`.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible answers.
12. `12-Quick-Revision.md` — High-density RBAC and security cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 11 - Autoscaling](../11-Auto-Scaling-HPA-VPA-Cluster-Autoscaler/README.md) | [README](./README.md) | [01 - Authentication Architecture](./01-Kubernetes-Authentication-X509-OIDC-and-Tokens.md) |
