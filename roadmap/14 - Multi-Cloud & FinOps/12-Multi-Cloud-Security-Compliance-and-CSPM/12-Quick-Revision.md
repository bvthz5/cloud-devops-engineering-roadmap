# Quick Revision Notes - Multi-Cloud Security, Compliance & CSPM

> **Module**: Multi-Cloud Security, Compliance & CSPM

---

## ⚡ Key Cheat Sheet

| Tool / Technology | Primary Purpose | Core Highlight |
|---|---|---|
| **Cloud Custodian** | Waste reduction & automated compliance | YAML-based rules engine for AWS, Azure, GCP |
| **OpenTelemetry (OTel)** | Vendor-neutral telemetry standardization | OTLP protocol, Collector architecture |
| **OpenTofu** | Open-source IaC framework | Open-source MPL-licensed Terraform fork |
| **Terragrunt** | DRY IaC architecture management | Inherits backends and providers across accounts |
| **CSPM (Wiz/Prisma)** | Cloud Security Posture Management | Continuous vulnerability and misconfiguration scanning |

---

## 📝 Top 5 Rules to Remember
1. **Automate waste remediation with Cloud Custodian** (delete unattached disks, release unassociated EIPs).
2. **Use OpenTelemetry for vendor-neutral tracing and metrics** to prevent APM lock-in.
3. **Maintain DRY IaC using Terragrunt or OpenTofu modules**.
4. **Use Memory Limiter processors in OTel Collectors** to prevent OOMKilled crashes during log spikes.
5. **Schedule automated weekend shutdowns for dev/test environments** to achieve immediate 65% compute savings.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Section (15 - CI-CD Pipelines & Automation) →](../../15%20-%20CI-CD%20Pipelines%20%26%20Automation/01-CI-CD-Foundations-and-Best-Practices/01-Continuous-Integration-vs-Continuous-Delivery-vs-Continuous-Deployment.md) |
