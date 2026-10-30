# 01 - Commit Spoofing Threat Model and Identity

## 1. The Glaring Flaw in Git Identity

Git was designed for trusting open-source kernel developers in 2005. **Git performs zero identity verification by default.**
Any developer can run:

```bash
git config user.name "Satya Nadella"
git config user.email "satya@microsoft.com"
git commit -m "malicious commit"
```
When pushed to GitHub, GitHub inspects the email header and displays Satya Nadella's photo and profile!

---

## 2. Supply Chain Impact
Attackers impersonate trusted maintainers, slip vulnerabilities into dependencies, and bypass basic review scrutiny.
**The Solution:** Cryptographically signed commits. Unsigned commits are marked as unverified and rejected by enterprise branch protection rules.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (09-Monorepos-Submodules-and-Large-Scale-Git)](../09-Monorepos-Submodules-and-Large-Scale-Git/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Cryptographic Signing with GPG and SSH Keys →](./02-Cryptographic-Signing-with-GPG-and-SSH-Keys.md) |
