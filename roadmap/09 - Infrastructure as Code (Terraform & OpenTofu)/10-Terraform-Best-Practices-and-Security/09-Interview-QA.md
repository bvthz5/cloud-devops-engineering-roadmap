# 09 - Interview Questions

### Q1: How do you prevent secrets from being exposed in Terraform state files?
**Answer:**
1. Use external secret managers (Vault, AWS Secrets Manager, SSM) via data sources
2. Mark outputs as `sensitive = true`
3. Encrypt state at rest (S3 SSE-KMS, GCS CMEK)
4. Restrict IAM access to state bucket
5. Use OpenTofu client-side state encryption
6. Never hardcode secrets in .tf files

---

### Q2: What is the difference between tfsec and Checkov?
**Answer:**
Both are static security scanners for Terraform. tfsec (by Aqua Security, now part of Trivy) focuses on security misconfigurations with simple rule IDs. Checkov (by Bridgecrew/Palo Alto) offers broader compliance frameworks (CIS, SOC2, HIPAA), supports custom Python checks, and scans additional IaC formats (CloudFormation, Kubernetes, Helm).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
