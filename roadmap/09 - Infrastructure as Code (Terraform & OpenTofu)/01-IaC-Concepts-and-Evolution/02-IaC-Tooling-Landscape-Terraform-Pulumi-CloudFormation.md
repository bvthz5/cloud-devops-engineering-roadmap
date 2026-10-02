# 02 - IaC Tooling Landscape: Terraform, Pulumi, CloudFormation & More

## 1. Comprehensive Tool Comparison

| Tool | Language | Paradigm | Multi-Cloud | State | License |
|---|---|---|---|---|---|
| **Terraform** | HCL | Declarative | ✅ Yes | Remote / Local | BSL 1.1 (post v1.6) |
| **OpenTofu** | HCL | Declarative | ✅ Yes | Remote / Local | MPL 2.0 (Open Source) |
| **Pulumi** | Python/Go/TS/C# | Imperative | ✅ Yes | Pulumi Cloud / S3 | Apache 2.0 |
| **AWS CloudFormation** | JSON/YAML | Declarative | ❌ AWS only | AWS-managed | Proprietary |
| **Azure Bicep** | Bicep DSL | Declarative | ❌ Azure only | Azure-managed | MIT |
| **Google Deployment Mgr** | YAML/Jinja/Python | Declarative | ❌ GCP only | GCP-managed | Proprietary |
| **Crossplane** | YAML (K8s CRDs) | Declarative | ✅ Yes | etcd (K8s) | Apache 2.0 |
| **CDK for Terraform** | TypeScript/Python | Imperative | ✅ Yes | Remote / Local | MPL 2.0 |
| **Ansible** | YAML | Declarative+Proc | ✅ Yes | Stateless | GPL 3.0 |

## 2. When to Choose What

```text
Need multi-cloud?
├── YES → Terraform/OpenTofu, Pulumi, or Crossplane
└── NO  → AWS? → CloudFormation or CDK
          Azure? → Bicep
          GCP? → Deployment Manager or Terraform

Need general-purpose language?
├── YES → Pulumi (Python, TypeScript, Go, C#, Java)
└── NO  → Terraform/OpenTofu (HCL — purpose-built DSL)

Need Kubernetes-native control plane?
├── YES → Crossplane (manages cloud resources as K8s CRDs)
└── NO  → Terraform/OpenTofu
```

## 3. Terraform vs OpenTofu — The Fork Story

- **August 2023:** HashiCorp relicensed Terraform from MPL 2.0 to BSL 1.1 (Business Source License)
- **September 2023:** OpenTofu manifesto signed by 100+ companies; forked from Terraform v1.5.7
- **January 2024:** OpenTofu v1.6.0 released under Linux Foundation stewardship
- **Compatibility:** OpenTofu maintains backward compatibility with Terraform HCL; `tofu` CLI is a drop-in replacement for `terraform`

## 4. The IaC Ecosystem Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                     IaC ECOSYSTEM                           │
│                                                             │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐              │
│  │ Terraform │  │  OpenTofu │  │  Pulumi   │   ENGINES    │
│  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘              │
│        │               │               │                    │
│        └───────┬───────┘               │                    │
│                ▼                       ▼                    │
│  ┌──────────────────────┐  ┌────────────────────┐          │
│  │  HCL Configuration   │  │ Python/Go/TS Code  │  CODE    │
│  └──────────┬───────────┘  └────────┬───────────┘          │
│             ▼                       ▼                       │
│  ┌─────────────────────────────────────────────┐           │
│  │           PROVIDER / SDK LAYER              │           │
│  │  AWS │ Azure │ GCP │ K8s │ Datadog │ ...    │           │
│  └─────────────────────┬───────────────────────┘           │
│                        ▼                                    │
│  ┌─────────────────────────────────────────────┐           │
│  │           CLOUD / ON-PREM APIs              │  TARGET   │
│  └─────────────────────────────────────────────┘           │
└─────────────────────────────────────────────────────────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - What Is IaC](./01-What-Is-IaC-Declarative-vs-Imperative.md) | [README](./README.md) | [03 - Mutable vs Immutable](./03-Mutable-vs-Immutable-Infrastructure.md) |
