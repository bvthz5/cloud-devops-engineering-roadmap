# 04 - Secret Scanning and Credential Leak Prevention

## 1. The Threat of Leaked Credentials

Automated scanners (Shodan, GitGuardian, malicious bots) scrape GitHub public commits within **1.5 seconds** of a push. Committing an AWS Access Key (`AKIA...`) results in crypto miners launching thousands of GPU instances within minutes!

---

## 2. Automated Prevention Tools

- **Gitleaks:** Blazing fast Go-based tool with regex patterns for 150+ cloud providers and SaaS tokens.
- **Trufflehog:** Scans for high-entropy strings and actively verifies whether discovered secrets are currently live!

```bash
# Scan entire Git history for leaked secrets
gitleaks detect --verbose

# Run Gitleaks directly on staged files before commit
gitleaks protect --staged
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - The pre commit Framework Standardization](./03-The-pre-commit-Framework-Standardization.md) | [Index](../../../README.md) | [05 - Server Side Hooks pre receive and post receive →](./05-Server-Side-Hooks-pre-receive-and-post-receive.md) |
