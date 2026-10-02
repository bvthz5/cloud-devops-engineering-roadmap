# Module 10: Git Security, Cryptographic Signing, and Supply Chain Integrity

Welcome to **Module 10: Git Security, Cryptographic Signing, and Supply Chain Integrity**. In modern software supply chain attacks, malicious actors spoof commit author identities or inject backdoors. Cryptographic verification and historical auditing form an organization's first line of defense.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Understand the **Git Identity Spoofing Threat** and why `user.name` provides zero authentication.
2. Sign commits and tags cryptographically using **GPG keys** and modern **SSH signing keys**.
3. Enforce **Commit Verification** and signed commit requirements in GitHub and GitLab branch policies.
4. Surgically purge leaked passwords, API keys, and sensitive data from Git history using **`git filter-repo`** (replacing deprecated `git filter-branch`).
5. Comply with the **SLSA (Supply-chain Levels for Software Artifacts)** framework and generate verifiable commit provenance.
6. Audit repository security using automated scanning and policy enforcement.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Commit Spoofing Threat Model](./01-Commit-Spoofing-Threat-Model-and-Identity.md) | How easy commit impersonation is, supply chain attack vectors |
| 02 | [Cryptographic Signing: GPG vs. SSH](./02-Cryptographic-Signing-with-GPG-and-SSH-Keys.md) | GPG setup, modern SSH key commit signing (Git 2.34+), Sigstore Cosign |
| 03 | [Enforcing Signed Commits](./03-Enforcing-Signed-Commits-in-Branch-Protection.md) | GitHub Verified badges, rejecting unsigned pushes on servers |
| 04 | [Purging Leaked Secrets: git filter-repo](./04-Purging-Leaked-Secrets-with-git-filter-repo.md) | Why `git rm` fails, BFG Repo-Cleaner vs `git filter-repo`, history rewriting |
| 05 | [Supply Chain Security & SLSA](./05-Supply-Chain-Security-SLSA-Framework-and-SBOMs.md) | Tamper-proof pipelines, SLSA levels, Software Bill of Materials (SBOM) |
| 06 | [Repo Auditing & Access Governance](./06-Repository-Auditing-and-Access-Governance.md) | Deploy keys, Personal Access Tokens (PAT) vs Fine-Grained Tokens, SAML SSO |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Impersonated commit injecting backdoor, leaked production private key purge |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Fixing `gpg: signing failed: secret key not available`, SSH pinentry errors |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Configuring SSH key commit signing and verifying signatures locally |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Signing setup cheat sheet, filter-repo command syntax, security checklist |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Monorepos & Large Git](../09-Monorepos-Submodules-and-Large-Scale-Git/README.md) | [README](./README.md) | [01 - Spoofing Threat](./01-Commit-Spoofing-Threat-Model-and-Identity.md) |
