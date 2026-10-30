# Git & Version Control Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Massive Git Repository Size Sluggishness from 10GB of Binary Blobs

### 🚨 The Production Scenario
A legacy repository takes 45 minutes to clone (`git clone`). The `.git` directory is 18GB, despite source code files only totaling 50MB.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Binary artifacts (compiled `.tar.gz`, Docker tarballs, machine learning models, `.mp4` video assets) were checked directly into Git over several years. Because Git computes compression deltas efficiently for text but poorly for binary blobs, every version of a 100MB binary is stored as an independent uncompressible blob in packfiles.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Identify the largest objects in Git history using `git verify-pack`.
- Step 2: Install and configure Git Large File Storage (Git LFS) or an external artifact registry (Nexus/Artifactory/S3).
- Step 3: Use `git lfs migrate import --include='*.iso,*.bin,*.tar.gz,*.zip'` to convert historical commits to LFS pointer files.
- Step 4: Prune dangling objects with `git gc --prune=now --aggressive`.
- Step 5: Configure `.gitattributes` to route future binary file extensions automatically to Git LFS.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List the top 10 largest historical binary blobs committed anywhere in the repository
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | awk '$1=="blob" {print $2, $3, $4}' | sort -k2 -n | tail -10

# Convert all historical binary files to lightweight Git LFS text pointers across all branches
git lfs migrate import --everything --include='*.tar.gz,*.zip,*.bin'

# Purge all unreferenced blob packfiles and reclaim disk space immediately
git reflog expire --expire=now --all && git gc --prune=now --aggressive

# Configure .gitattributes to ensure new binary commits store pointers instead of raw blobs
git lfs track '*.zip' '*.tar.gz'

# Push rewritten streamlined commit history to remote repository
git push origin --force --all

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Git is designed for text deltas; committing binary artifacts balloons repository packfiles and makes CI clone times unmanageable. I identify top offenders using `git rev-list --objects --all`, migrate historical binaries into Git LFS using `git lfs migrate import`, purge dead packfiles with `git gc --aggressive --prune=now`, and configure `.gitattributes` and pre-commit checks to reject files over 50MB, redirecting builds to pull artifacts from Artifactory or S3 instead."

---

## 📌 Scenario 5: Merge Conflicts in 'package-lock.json' or Database Migration Scripts

### 🚨 The Production Scenario
During a sprint release, two parallel engineering teams commit database migration scripts with conflicting sequence numbers, plus 2,000 lines of merge conflicts in `package-lock.json`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Package lock files are generated automatically by package managers with complex internal dependency graphs; manually resolving a 2,000-line conflict introduces subtle syntax or integrity checksum errors. In database migrations, sequential migration files (`V12__add_table.sql` vs `V12__update_user.sql`) cause runtime collisions in Flyway/Liquibase migration engines.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: For lock files, checkout the destination branch's lock file and regenerate cleanly via `npm install` or `yarn install`.
- Step 2: For database migrations, renumber the newer migration file to take the next sequential integer.
- Step 3: Update Flyway/Liquibase schema version table entries.
- Step 4: Enable `git rerere` (Reuse Recorded Resolution) to automate resolving identical conflicts across rebases.
- Step 5: Run integration tests against a clean database instance to verify migration continuity.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Safely regenerate package-lock.json resolving all internal checksum conflicts automatically
git checkout --theirs package-lock.json && npm install

# Enable Git Reuse Recorded Resolution to auto-resolve repeating merge conflicts during rebasing
git config --global rerere.enabled true

# List all files currently in an Unmerged (conflict) state
git diff --name-only --diff-filter=U

# Inspect conflict markers and staged resolution status
git status

# Commit the cleanly tested resolution
git commit -m 'chore: resolve lockfile conflict and renumber migration v13'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Never manually edit a 2,000-line lockfile conflict. The industry best practice is to checkout the base or feature lockfile (`git checkout --theirs package-lock.json`) and run `npm install` to allow the package manager engine to resolve the dependency graph and SHA-512 hashes deterministically. For migration scripts, I coordinate renumbering the version prefix to the next unused sequence number, test against a local containerized DB, and enable `git rerere` to speed up future branch rebases."

---

## 📌 Scenario 6: Git Monorepo Sluggishness and Slow CI/CD Pipeline Triggers

### 🚨 The Production Scenario
An enterprise monorepo containing 80 backend services takes 5 minutes to run `git status` on developer laptops, and CI builds all 80 microservices whenever a single markdown file changes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Standard Git scans every file in the working directory during `git status`. As file counts exceed 500,000, filesystem stat calls overload OS caches. In CI, naive push triggers lack path-filtering, treating any repo change as a global build trigger.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Enable Git File System Monitor (`core.fsmonitor`) and untracked cache (`core.untrackedCache`).
- Step 2: Use Git Sparse Checkout to allow developers to check out only their specific service directory.
- Step 3: Implement Git Scalar (`scalar register .`) for enterprise repository optimization.
- Step 4: Configure CI/CD path-filtering rules (e.g., GitHub Actions `paths: ['services/auth/**']` or Nx/Turborepo dependency graph).
- Step 5: Measure `git status` duration drop from 300s to <1s.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Enable built-in OS file system event monitoring to accelerate git status on large codebases
git config core.fsmonitor true && git config core.untrackedcache true

# Initialize sparse checkout in high-performance cone mode
git sparse-checkout init --cone

# Mount only the designated service folders and dependencies into the local working directory
git sparse-checkout set services/payment-service shared/libs

# Enable Microsoft/Git Scalar monorepo management enhancements
scalar register .

# Verify instantaneous git status execution on monorepo
git status

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Monorepo scaling requires tooling at both the local client level and the CI level. Locally, I enable `core.fsmonitor` and use `git sparse-checkout --cone` so engineers only download and track their active service subdirectories. In CI/CD, I implement path-based change detection with tools like Nx, Turborepo, or GitHub Actions path filters to build and test only the microservices affected by the commit's diff graph."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Git & Version Control Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Git & Version Control Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

