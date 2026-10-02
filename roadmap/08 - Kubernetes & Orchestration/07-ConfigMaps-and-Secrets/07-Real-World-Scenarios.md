# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Committed Production API Key Leak

### Incident Summary
A developer accidentally committed a production Stripe secret key inside a Kubernetes Secret YAML manifest to a public GitHub repository. Within 12 minutes, automated scrapers drained $85,000 via fraudulent payment intents.

### Root Cause
No pre-commit scanning was configured on developer workstations, and Secrets were managed manually instead of using external secret stores.

### Remediation
1. **Immediate Revocation:** The Stripe key was rotated immediately from Stripe dashboard.
2. **GitGuardian & Gitleaks:** Enforced pre-commit hooks and GitHub Actions workflows blocking commits containing regex matches for credentials.
3. **Migrated to External Secrets Operator:** Completely removed native Secret manifests from all git repositories.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Reloader Automation](./06-Reloader-Automatic-Pod-Rollout-on-Config-Changes.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
