# Git & Version Control Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Commit Signing Verification (GPG/SSH) Failing in Release Pipelines

### 🚨 The Production Scenario
An automated GitHub Actions semantic release pipeline fails because the repository enforces signed commits, and the automated release bot's commits are marked 'Unverified'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Enterprise repositories enforce branch protection requiring cryptographically signed commits (GPG, SSH, or Sigstore/Cosign). If the automated CI runner uses a generic `GITHUB_TOKEN` without an associated GPG/SSH signing key configured in Git config, commits fail branch protection validation.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Generate a dedicated GPG or SSH signing key for the CI service account.
- Step 2: Add the public key to GitHub/GitLab Organization Verified Keys.
- Step 3: Store the encrypted private key and passphrase in CI secrets (e.g. GitHub Secrets or HashiCorp Vault).
- Step 4: Configure the CI workflow to import the key and set `git config commit.gpgsign true`.
- Step 5: Alternatively, use GitHub App tokens which automatically receive GitHub's verified bot signature.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Configure default GPG key ID for commit signing
git config --global user.signingkey <GPG_KEY_ID>

# Enforce automatic signing on all created commits
git config --global commit.gpgsign true

# Verify the cryptographic signature and trust validity on the latest commit
git log --show-signature -1

# Export public key to register within GitHub/GitLab verified keys settings
gpg --armor --export <GPG_KEY_ID>

# Modern alternative: configure lightweight SSH keys for Git commit signing
git config --global gpg.format ssh

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When enterprise repos enforce signed commits, automated CI bots must either sign with a dedicated imported GPG/SSH key or use a GitHub App token. Using GitHub App tokens is best practice because GitHub automatically attaches an official verified cryptographic signature to bot commits without requiring fragile private GPG keys to be stored in CI secrets."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Production Hotfix Needed While Feature Branch Has 50 Unrelated Commits** | `git checkout -b hotfix/cve-patch main` | Feature branches diverge significantly from main. Merging the entire feature bra... |
| **Scenario 2: Accidental Secret Committed and Pushed to a Public/Shared Git Repository** | `aws iam delete-access-key --user-name dev-admin --access-key-id AKIAIOSFODNN7EXAMPLE` | Simply deleting the file in a new commit (`git rm`) does not remove it from Git'... |
| **Scenario 3: Developer Accidental 'git push --force' Overwriting Main Branch History** | `git reflog show origin/main` | A force push overwrites the remote branch reference pointer (`refs/heads/main`).... |
| **Scenario 4: Massive Git Repository Size Sluggishness from 10GB of Binary Blobs** | `git rev-list --objects --all \| git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' \| awk '$1=="blob" {print $2, $3, $4}' \| sort -k2 -n \| tail -10` | Binary artifacts (compiled `.tar.gz`, Docker tarballs, machine learning models, ... |
| **Scenario 5: Merge Conflicts in 'package-lock.json' or Database Migration Scripts** | `git checkout --theirs package-lock.json && npm install` | Package lock files are generated automatically by package managers with complex ... |
| **Scenario 6: Git Monorepo Sluggishness and Slow CI/CD Pipeline Triggers** | `git config core.fsmonitor true && git config core.untrackedcache true` | Standard Git scans every file in the working directory during `git status`. As f... |
| **Scenario 7: Bisecting a Production Regression Introduced Across 600 Commits** | `git bisect start HEAD v1.4.0` | Linear search through 600 commits would require dozens of manual builds and depl... |
| **Scenario 8: Submodule Pointer Desynchronization Breaking CI/CD Builds** | `git submodule status` | Git submodules track an explicit commit SHA in the parent repository, not a bran... |
| **Scenario 9: Uncommitted Local Work Blocking Urgent Production Hotfix Pull** | `git stash push -u -m 'temporary-bastion-changes'` | Git safeguards against silent data loss by refusing to overwrite modified tracke... |
| **Scenario 10: Commit Signing Verification (GPG/SSH) Failing in Release Pipelines** | `git config --global user.signingkey <GPG_KEY_ID>` | Enterprise repositories enforce branch protection requiring cryptographically si... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Git & Version Control Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Containers & Docker Interview Scenarios: Production Incidents & Triage Scenarios →](../04-Containers-and-Docker-Interview-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

