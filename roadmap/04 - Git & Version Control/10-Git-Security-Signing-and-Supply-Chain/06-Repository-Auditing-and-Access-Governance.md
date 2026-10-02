# 06 - Repository Auditing and Access Governance

## 1. Access Governance Best Practices

- **Personal Access Tokens (PAT):** Never use legacy tokens with infinite lifespan. Mandate **Fine-Grained PATs** with expiration $< 90$ days restricted to specific repositories and permissions.
- **Deploy Keys:** Read-only SSH keys provisioned for automated server clones.
- **SAML SSO & SCIM:** Synchronize user repository permissions with corporate Okta / Azure AD.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Supply Chain & SLSA](./05-Supply-Chain-Security-SLSA-Framework-and-SBOMs.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
