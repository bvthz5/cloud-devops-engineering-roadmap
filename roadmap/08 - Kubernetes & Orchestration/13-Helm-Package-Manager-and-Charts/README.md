# 13 - Helm Package Manager and Charts

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Helm-v3-Architecture-and-Release-Lifecycle.md` — The package manager for Kubernetes: Tillerless Helm v3 architecture, Secrets-based state storage, and release 3-way merge.
2. `02-Helm-Chart-Directory-Structure-and-Chart-yaml.md` — Anatomy of a Helm chart: `Chart.yaml`, `values.yaml`, `templates/`, `_helpers.tpl`, and `.helmignore`.
3. `03-Go-Templates-Values-yaml-and-Built-in-Objects.md` — Template logic: Sprig template functions, pipelines, control flow (`if/else`, `range`, `with`), and scopes.
4. `04-Subcharts-Chart-Dependencies-and-Library-Charts.md` — Enterprise modularity: child charts, global values, condition-based chart inclusion, and reusable Library charts.
5. `05-Helm-Hooks-and-Lifecycle-Management.md` — Intercepting releases: `pre-install`, `post-install`, `pre-upgrade`, `post-rollback`, and database migration hooks.
6. `06-OCI-Chart-Registries-and-Enterprise-Distribution.md` — Distributing charts as OCI artifacts: `helm push` to AWS ECR, Harbor, or GitHub Packages.
7. `07-Real-World-Scenarios.md` — Production post-mortems: failed Helm upgrade leaving release in `pending-upgrade` deadlock, and Jinja/Go template injection.
8. `08-Troubleshooting.md` — Diagnostic runbook for Helm release deadlocks, template rendering errors (`helm template --debug`), and hook timeouts.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Helm package management.
10. `10-Hands-On-Practice.md` — Production lab: writing a reusable microservice chart with helpers, schema validation, and lifecycle hooks.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density Helm CLI and template syntax reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 12 - RBAC & Security](../12-RBAC-and-Cluster-Security/README.md) | [README](./README.md) | [01 - Helm v3 Architecture](./01-Helm-v3-Architecture-and-Release-Lifecycle.md) |
