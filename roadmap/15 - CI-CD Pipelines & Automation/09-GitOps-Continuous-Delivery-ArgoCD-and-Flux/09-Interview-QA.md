# Interview Q&A - GitOps & Continuous Delivery (ArgoCD & Flux)

> **Module**: GitOps & Continuous Delivery (ArgoCD & Flux)

---

### Q1: What is the primary difference between Push-Based CI/CD and Pull-Based GitOps?
**Answer**:
- **Push-Based**: The CI pipeline runner (e.g., Jenkins or GitHub Actions) executes `kubectl apply` or `helm upgrade` against the Kubernetes API server using stored credentials.
- **Pull-Based (GitOps)**: An agent running *inside* the cluster (e.g., ArgoCD or Flux) continuously polls the Git repository and pulls changes into the cluster. This eliminates external cluster admin credentials and automatically heals manual cluster drift.

---

### Q2: What is an SBOM (Software Bill of Materials) and why is it essential for DevSecOps?
**Answer**:
An SBOM is a formal, machine-readable inventory of all third-party libraries, modules, and open-source packages included within a container image or software application (formats like CycloneDX or SPDX). It allows security teams to instantly query whether new zero-day vulnerabilities (e.g., Log4j) affect their deployed software inventory across production environments.

---

### Q3: How do Feature Flags decouple deployment from release?
**Answer**:
- **Deployment**: The technical action of pushing code to production servers or Kubernetes Pods.
- **Release**: The business action of making features accessible to end users.
Feature flags wrap new code branches in conditional toggles. Developers can deploy code to production daily while keeping the feature flag turned OFF, enabling safe testing in production and instant instant feature rollouts without redeploying code.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
