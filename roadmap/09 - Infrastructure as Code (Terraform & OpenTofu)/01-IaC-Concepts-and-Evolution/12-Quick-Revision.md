# 12 - Quick-Revision & Enterprise Cheat Sheet

## IaC Core Concepts at a Glance

| Concept | Key Point |
|---|---|
| **Declarative** | Describe end state; tool computes diff |
| **Imperative** | Describe step-by-step instructions |
| **Idempotency** | Apply N times = same result as apply once |
| **Configuration Drift** | Actual state ≠ defined state (caused by manual changes) |
| **Immutable Infra** | Never modify running instances; replace with new images |
| **Mutable Infra** | In-place updates; risk of snowflake servers |
| **Golden Image** | Pre-baked machine image (Packer → AMI/image) |
| **Blast Radius** | Scope of impact from a single change |

## IaC Tool Quick Reference

| Tool | Language | Multi-Cloud | State | License |
|---|---|---|---|---|
| Terraform | HCL | ✅ | File/Remote | BSL 1.1 |
| OpenTofu | HCL | ✅ | File/Remote | MPL 2.0 |
| Pulumi | Python/Go/TS | ✅ | Cloud/S3 | Apache 2.0 |
| CloudFormation | YAML/JSON | ❌ (AWS) | AWS | Proprietary |
| Bicep | Bicep DSL | ❌ (Azure) | Azure | MIT |
| Crossplane | K8s YAML | ✅ | etcd | Apache 2.0 |

## IaC Lifecycle Commands

```bash
terraform init         # Download providers, initialize backend
terraform fmt          # Format code to canonical style
terraform validate     # Check syntax and internal consistency
terraform plan         # Preview changes (dry run)
terraform apply        # Execute changes
terraform destroy      # Tear down all managed resources
terraform import       # Import existing resource into state
terraform state list   # List all resources in state
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 02 - Terraform Architecture & CLI](../02-Terraform-Architecture-and-CLI/README.md) |
