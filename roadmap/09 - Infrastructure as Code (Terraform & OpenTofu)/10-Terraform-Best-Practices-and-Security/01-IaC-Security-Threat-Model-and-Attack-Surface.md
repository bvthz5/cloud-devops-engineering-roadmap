# 01 - IaC Security Threat Model & Attack Surface

## 1. IaC Attack Vectors

| Vector | Risk | Mitigation |
|---|---|---|
| Secrets in .tf files | Credentials exposed in Git | Use variables with sensitive flag + external secret manager |
| State file exposure | All attributes in plaintext JSON | Encrypt state, restrict access, use remote backend |
| Overly permissive IAM | Terraform role has admin access | Least-privilege IAM policies |
| Unsigned providers | Malicious provider binary | Use lock file, verify GPG signatures |
| Public S3 buckets in config | Data exposure | tfsec / Checkov scanning |
| Unencrypted resources | Data at rest exposure | Policy-as-code enforcement |

## 2. Security Scanning Pipeline

```text
Developer writes .tf -> Pre-commit hooks (tfsec, checkov)
    |
    v
PR created -> CI runs: fmt, validate, tflint, tfsec, checkov
    |
    v
Plan generated -> Conftest/OPA policy check on plan JSON
    |
    v
Human review -> Check for sensitive changes
    |
    v
Apply (with approval gate in production)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - tfsec](./02-tfsec-Static-Security-Scanner.md) |
