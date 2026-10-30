# 08 - Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| tfsec finding false positive | Rule doesn't apply to use case | Add inline `#tfsec:ignore:RULE_ID` with justification |
| Checkov scan too slow | Scanning entire repo | Use `--directory` flag to target specific modules |
| Vault token expired | Short-lived token | Use AppRole or Kubernetes auth method |
| SOPS decryption failure | Wrong KMS key or missing IAM access | Verify KMS key ARN and IAM permissions |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
