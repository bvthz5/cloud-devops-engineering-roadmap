# Terragrunt: DRY Infrastructure Code Across AWS, Azure & GCP

> **Module**: Multi-Cloud Infrastructure Automation with Terraform & OpenTofu  
> **Level**: Enterprise FinOps & Multi-Cloud Architecture  

---

## 📌 Executive Summary
Understanding **Terragrunt: DRY Infrastructure Code Across AWS, Azure & GCP** is fundamental for cloud architects and DevOps engineers leading multi-cloud strategies and FinOps transformations. This guide covers core principles, cross-cloud tooling, cost allocation taxonomies, and production implementation models.

---

## 🏗 Architecture & Core Mechanics

```
+-------------------------------------------------------------------------------+
|                       FINOPS LIFECYCLE & MULTI-CLOUD SCOPE                    |
+-------------------------------------------------------------------------------+
|   INFORM (Visibility & Tags)  -->  OPTIMIZE (Rates & Sizing)  --> OPERATE     |
|   AWS Cost Explorer  |  Azure Cost Mgmt  |  GCP BigQuery  | Kubecost / FOCUS  |
+-------------------------------------------------------------------------------+
```

### Key Pillars
1. **Multi-Cloud Automation**: Orchestrating declarative infrastructure across AWS, Azure, and GCP using Terraform, OpenTofu, and Terragrunt.
2. **Open Telemetry & Observability**: Vendor-neutral tracing, logging, and metrics aggregation with OpenTelemetry (OTel), Prometheus, and Grafana.
3. **Automated Waste Remediation**: Eliminating unattached EBS/Managed Disks, idle VMs, and unassociated Elastic IPs using Cloud Custodian policies.
4. **Unified Cloud Security Posture**: Managing Multi-Cloud CSPM, CIS compliance benchmarks, and Policy-as-Code guardrails centrally.

---

## 🛠 Automation & Configuration Examples

### Cloud Custodian Policy Deleting Unattached Elastic IPs Across Regions
```yaml
policies:
  - name: aws-delete-unassociated-eips
    resource: aws.eip
    filters:
      - association_id: empty
    actions:
      - type: release
```

### Terraform Multi-Provider Configuration (AWS + Azure + GCP)
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

provider "google" {
  region = "us-central1"
}
```

---

## 💡 Industry Best Practices
- **Use OpenTofu / Terraform for Multi-Provider Declarative Code**: Keep module interfaces consistent across cloud providers.
- **Implement Cloud Custodian for Waste Governance**: Run automated hourly/daily policies to clean up orphaned disks, snapshot back-ups, and unattached IPs.
- **Standardize on OpenTelemetry (OTel)**: Decouple telemetry collection from vendor lock-in; export logs/metrics to any APM backend.
- **Enforce Centralized CSPM Compliance**: Use Wiz, Prisma Cloud, or Security Command Center to maintain continuous CIS benchmark audit readiness.

---

## 🔗 Related Resources
- [OpenTofu Official Documentation](https://opentofu.org/)
- [Cloud Custodian Documentation](https://cloudcustodian.io/)
