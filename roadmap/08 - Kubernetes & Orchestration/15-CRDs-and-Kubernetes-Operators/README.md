# 15 - CRDs and Kubernetes Operators

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-CustomResourceDefinitions-CRDs-and-OpenAPI-v3-Validation.md` — Extending the Kubernetes API: schema definitions, versions (`v1alpha1`, `v1`), structural schemas, and validation.
2. `02-The-Kubernetes-Operator-Pattern-and-Control-Loop.md` — Human operational knowledge encoded in software: the Level-Triggered reconciliation loop.
3. `03-Building-Operators-with-Kubebuilder-and-Controller-Runtime.md` — Developing operators in Go: scaffolding with Kubebuilder, markers, and code generators.
4. `04-Operator-Reconciliation-Loops-and-Event-Handling.md` — Writing the `Reconcile()` method: watches, predicates, rate limiting, and exponential requeue.
5. `05-Finalizers-and-Safe-Resource-Teardown.md` — Graceful cleanup: `metadata.finalizers`, external resource deletion (cloud databases, DNS records), and deadlock prevention.
6. `06-Operator-Lifecycle-Manager-OLM-and-OperatorHub.md` — Distributing operators: OLM, `ClusterServiceVersion` (CSV), catalogs, subscriptions, and OperatorHub.
7. `07-Real-World-Scenarios.md` — Production post-mortems: the unremovable CRD finalizer deadlock, and runaway reconciliation API storms.
8. `08-Troubleshooting.md` — Diagnostic runbook for stuck custom resources, CRD conversion webhooks, and controller crash loops.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on the Operator pattern.
10. `10-Hands-On-Practice.md` — Production lab: defining a custom CRD, creating instances, and verifying schema validation rejections.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density CRD and Operator cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 14 - CNI & NetworkPolicies](../14-Kubernetes-Networking-CNI-and-NetworkPolicies/README.md) | [README](./README.md) | [01 - CRD Architecture](./01-CustomResourceDefinitions-CRDs-and-OpenAPI-v3-Validation.md) |
