# 07 - Real-World Scenarios

## Scenario 01: Secret Leaked in Terraform State

### Incident
A database password was hardcoded in a .tf file. The state file containing the plaintext password was stored in an S3 bucket with public read access.

### Resolution
1. Immediately rotated the database password
2. Made S3 bucket private with `Block Public Access`
3. Moved secrets to AWS Secrets Manager with data source lookups
4. Added tfsec to CI pipeline to catch future hardcoded secrets
5. Enabled S3 bucket encryption with KMS

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Supply Chain Security Provider Signing and Lock Files](./06-Supply-Chain-Security-Provider-Signing-and-Lock-Files.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
