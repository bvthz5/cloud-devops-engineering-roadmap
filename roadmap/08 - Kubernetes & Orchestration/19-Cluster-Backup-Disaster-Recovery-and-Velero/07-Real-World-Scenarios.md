# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Cloud Account Terraform Deletion

### Incident Summary
A rogue Terraform `destroy` pipeline executed with elevated permissions deleted an entire production EKS cluster and its underlying VPCs.

### Root Cause
An accidental merge to `main` triggered an automated destroy run in CI.

### Remediation
1. Terraform state was reapplied to rebuild VPC, subnets, and new EKS cluster control plane (Time: 18 minutes).
2. Velero was deployed, pointed to the replicated cross-region S3 backup bucket.
3. `velero restore create --from-backup prod-nightly` restored all 45 namespaces, deployments, and CSI volume snapshots in 11 minutes. Total downtime: 29 minutes (within 1-hour RTO SLA!).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - DR Testing & Validation](./06-DR-Testing-Validation-and-RTO-Benchmarking.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
