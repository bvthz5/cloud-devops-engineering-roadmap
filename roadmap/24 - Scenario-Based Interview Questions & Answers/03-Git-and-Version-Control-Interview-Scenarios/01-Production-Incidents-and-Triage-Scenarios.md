# Git & Version Control Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Production Hotfix Needed While Feature Branch Has 50 Unrelated Commits

### 🚨 The Production Scenario
A P0 vulnerability in production requires a one-line security fix present only inside an unreleased feature branch containing 50 incomplete, unstable commits.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Feature branches diverge significantly from main. Merging the entire feature branch into main would introduce untested code into production. Rebasing the feature branch onto main could destabilize active development.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check out a clean branch from the current production tag or main branch.
- Step 2: Identify the exact commit SHA of the security fix using `git log` or `git show`.
- Step 3: Cherry-pick the isolated commit using `git cherry-pick <COMMIT_SHA>`.
- Step 4: Verify that only the intended diff is applied using `git diff HEAD~1` and run the test suite.
- Step 5: Push the hotfix branch, merge via fast-forward or squash, and deploy immediately.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Create an isolated hotfix branch directly from current production main
git checkout -b hotfix/cve-patch main

# Search the feature branch commit log to locate the exact patch SHA
git log --grep='fix security' feature/next-gen-v2

# Apply the specific commit cleanly, recording original commit reference in commit message
git cherry-pick -x <COMMIT_SHA>

# Carefully audit the staged changes to ensure no extraneous code or dependencies leaked in
git diff HEAD~1

# Push the single-commit hotfix branch to initiate production CI/CD deployment
git push origin hotfix/cve-patch

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "To isolate a critical fix without bringing in unreleased code, I create a hotfix branch branched off `main`, locate the target commit SHA, and run `git cherry-pick -x <SHA>`. The `-x` flag appends provenance so audit trails stay clean. I verify the diff with `git diff HEAD~1`, run automated unit tests, and promote the single commit straight through production pipelines, later merging main back into the feature branch."

---

## 📌 Scenario 2: Accidental Secret Committed and Pushed to a Public/Shared Git Repository

### 🚨 The Production Scenario
A developer accidentally committed and pushed an AWS root access key and private SSH key to a shared GitHub repository 5 commits ago.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Simply deleting the file in a new commit (`git rm`) does not remove it from Git's object database (`.git/objects/pack/`). The secret remains fully visible in commit history, diffs, and dangling commit trees to anyone who clones or fetches the repository.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: IMMEDIATELY revoke the exposed credentials in AWS IAM / Vault to eliminate live exploitation risk.
- Step 2: Coordinate with the team to pause active pushes to the repository.
- Step 3: Use `git-filter-repo` (modern standard) or BFG Repo-Cleaner to permanently purge the secret and file from all commits.
- Step 4: Force push the sanitized history across all branches using `git push --force --all` and `git push --force --tags`.
- Step 5: Expire GitHub/GitLab cached ref pointers and implement pre-commit hooks (`gitleaks`, `trufflehog`) to block future leaks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# First Priority: Revoke the active AWS credential immediately in cloud IAM
aws iam delete-access-key --user-name dev-admin --access-key-id AKIAIOSFODNN7EXAMPLE

# Install high-performance official Git history rewriting utility
pip install git-filter-repo

# Purge the exact secret string across all commits, blobs, and branches
git-filter-repo --replace-text <(echo 'AKIAIOSFODNN7EXAMPLE==>REDACTED')

# Force push rewritten commit history to remote repository
git push origin --force --all; git push origin --force --tags

# Run local secret detection to confirm zero residual credential patterns remain
gitleaks detect --source . -v

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The very first step is credential revocation in the cloud control plane — removing it from Git is secondary to eliminating active vulnerability. Once revoked, I use `git-filter-repo` to rewrite the packfiles across all branches and tags, force push the clean history, notify all contributors to re-clone, and enforce `pre-commit` hooks with `gitleaks` plus GitHub secret scanning to reject commits containing API tokens at the gate."

---

## 📌 Scenario 3: Developer Accidental 'git push --force' Overwriting Main Branch History

### 🚨 The Production Scenario
A developer accidentally executed `git push --force` to `main`, wiping out 3 weeks of merged pull requests and team commits.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A force push overwrites the remote branch reference pointer (`refs/heads/main`). However, Git is an append-only content-addressable storage engine; previous commits remain stored as 'dangling commits' in the local Git object store and can be retrieved using the reflog.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Identify an engineer or CI runner machine that recently had the latest version of `main`.
- Step 2: Inspect the local reflog using `git reflog show origin/main` or `git reflog` to locate the SHA prior to the forced push.
- Step 3: Point a recovery branch to that SHA: `git checkout -b recovery/main <REFLOG_SHA>`.
- Step 4: Push the recovered branch back to remote `main` using `git push --force-with-lease origin main`.
- Step 5: Immediately enable GitHub/GitLab Branch Protection Rules requiring PRs and completely disallowing force pushes on `main`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# View the timeline of previous remote branch commit pointers before the force push
git reflog show origin/main

# Inspect detailed commit metadata and messages stored in the local reflog database
git log -g -n 10

# Instantly checkout the previous good state prior to the forced update
git checkout -b restore-main HEAD@{1}

# Atomically restore remote main only if no other unrecorded changes occurred
git push --force-with-lease origin restore-main:main

# Enforce strict branch protection via GitHub CLI to prevent future force pushes
gh api -X PUT /repos/:owner/:repo/branches/main/protection -f enforce_admins=true

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Because Git commits are immutable and preserved in the reflog, code is almost never truly lost. I locate the commit SHA immediately prior to the force push using `git reflog show origin/main`, verify the tree, and restore the branch using `git push --force-with-lease`. Most importantly, I prevent recurrence by enabling branch protection rules on `main` with admin enforcement, requiring signed PR reviews and disallowing force pushes."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Computer Networking & DNS Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../02-Computer-Networking-and-DNS-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Git & Version Control Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

