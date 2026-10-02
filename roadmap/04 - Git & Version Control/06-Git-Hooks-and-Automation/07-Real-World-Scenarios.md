# 07 - Git Hooks: Real-World Production Scenarios

## Scenario 1: The $65,000 Leaked AWS Key Incident

### Incident Summary
A developer working late created a quick script to test AWS S3 uploads. They hardcoded `AWS_SECRET_ACCESS_KEY` and committed it. Because no pre-commit secret scanner was installed, the commit was pushed to a public repository. Within 4 minutes, automated bots detected the key and launched 120 GPU mining EC2 instances across 5 AWS regions, racking up $65,000 in charges before AWS automated fraud detection shut down the account.

### Resolution & Enforcement
1. The security team mandated the `pre-commit` framework with `gitleaks` across all internal repositories.
2. Enabled **GitHub Secret Scanning with Push Protection**, which actively evaluates every push and blocks it if high-confidence credentials are detected.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Bypassing Hooks](./06-Bypassing-Hooks-and-Security-Trade-Offs.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
