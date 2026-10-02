# 07 - Git Security: Real-World Production Scenarios

## Scenario 1: The Spoofed Maintainer Vulnerability Injection

### Incident Summary
A popular open-source Go package maintainer woke up to find a security advisory on their package. An attacker had pushed a commit containing an obfuscated backdoor with the author email set to the legitimate maintainer's email address.

### Root Cause
The repository did not enforce signed commits. The attacker spoofed the author metadata in the commit header.

### Resolution
The maintainer enabled GPG/SSH commit verification in GitHub branch settings. All unsigned PRs were automatically blocked, preventing future identity spoofing.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Repo Auditing](./06-Repository-Auditing-and-Access-Governance.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
