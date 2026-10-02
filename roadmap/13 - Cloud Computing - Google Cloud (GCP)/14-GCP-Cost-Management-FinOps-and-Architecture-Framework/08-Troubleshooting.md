# Troubleshooting Guide - GCP Cost Management, FinOps & Architecture Framework

> **Module**: GCP Cost Management, FinOps & Architecture Framework

---

## 🔍 Common Issue 1: VPC Service Controls Perimeter Denial (`vpcServiceControlsVpcssDenied`)

### Symptom
Applications or administrators receive `VPC Service Controls: Request denied by VPC Service Controls` error when querying BigQuery or GCS.

### Root Cause & Resolution
1. **Perimeter Audit**: Check Security Command Center or Cloud Audit Logs for `vpcServiceControlsVpcssDenied`.
2. **Access Level Verification**: Verify if the user request originates from an IP address or service account included in the perimeter Access Level.
3. **Ingress/Egress Policy**: Add explicit Ingress/Egress rules allowing the required API call between the external project and the protected perimeter project.

---

## 🔍 Common Issue 2: Terraform GCS State Lock Error

### Symptom
Executing `terraform apply` fails with `Error acquiring the state lock: GCS object locked`.

### Root Cause & Resolution
1. **Stale Lock**: A previous Terraform run was interrupted prematurely, leaving the state lock file active in the GCS backend bucket.
2. **Force Unlock**: Verify no other CI/CD pipeline or engineer is executing Terraform, then force unlock using the lock ID provided in the error message:
   ```bash
   terraform force-unlock LOCK_ID
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
