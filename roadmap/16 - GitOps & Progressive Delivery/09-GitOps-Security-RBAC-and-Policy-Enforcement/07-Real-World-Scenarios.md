# Real-World Scenarios - GitOps Security, RBAC & Policy Enforcement

> **Module**: GitOps Security, RBAC & Policy Enforcement

---

## 🏢 Scenario 1: Multi-Tenant Enterprise GitOps Governance with ArgoCD Projects & RBAC

### Background
A financial organization hosts 50 internal development teams on shared EKS/GKE clusters managed by a central platform engineering team. Developers need GitOps self-service capability but must be restricted from modifying cluster-wide CRDs or accessing other teams' namespaces.

### Solution Architecture
1. **ArgoCD Projects (`AppProject`)**: Define distinct ArgoCD AppProjects for each product squad (`project-payments`, `project-analytics`).
2. **Resource Whitelisting**: Configure `sourceRepos`, `destinations`, and `clusterResourceWhitelist` so teams can only deploy to designated namespaces and cannot alter cluster-scoped resources.
3. **SSO Integration**: Map Okta/Entra ID SAML groups to ArgoCD RBAC roles (`role:payments-admin`), providing seamless single sign-on access.

---

## 🏢 Scenario 2: GitOps Image Automation & Automated Version Commits

### Background
A SaaS company wants container images built by CI pipelines to be deployed to Kubernetes automatically without requiring developers to manually edit image tags in Git repositories.

### Solution Architecture
1. **Container Push Event**: CI pipeline builds Docker image `app:v2.1.4` and pushes it to Artifact Registry.
2. **Flux Image Automation Controller**: Flux scans Artifact Registry, detects new semantic version `v2.1.4` matching `ImagePolicy` (`semver: '>=2.1.0'`).
3. **Automated Git Commit**: Flux checks out the Git deployment repository, updates the image tag in `kustomization.yaml`, commits `[bot] Update image tag to v2.1.4`, and pushes back to Git. ArgoCD/Flux then reconciles the cluster to `v2.1.4`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Audit Logging Compliance and SOC2 Controls](./06-Audit-Logging-Compliance-and-SOC2-Controls.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
