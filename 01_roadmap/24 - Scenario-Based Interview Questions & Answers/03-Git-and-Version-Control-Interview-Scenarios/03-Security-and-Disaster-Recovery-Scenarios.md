# Git & Version Control Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Bisecting a Production Regression Introduced Across 600 Commits

### 🚨 The Production Scenario
A memory leak that degrades throughput after 2 hours of runtime was introduced into production sometime within the last 600 commits. Manual review is impossible.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Linear search through 600 commits would require dozens of manual builds and deploys. `git bisect` uses binary search (O(log n)), requiring only ~9 test iterations (2^9 = 512) to pinpoint the exact commit.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Identify a known good commit SHA (e.g. tag `v1.4.0`) and the current bad commit (`HEAD`).
- Step 2: Write an automated test script (`test.sh`) that exits with code 0 (good) or code 1 (bad).
- Step 3: Start the bisect session: `git bisect start HEAD v1.4.0`.
- Step 4: Automate the search using `git bisect run ./test.sh`.
- Step 5: Inspect the pinpointed commit diff, author, and PR for root-cause analysis.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Initiate binary search between known bad HEAD and known good release tag
git bisect start HEAD v1.4.0

# Execute fully automated test script to evaluate each commit automatically
git bisect run ./scripts/test_memory_leak.sh

# Inspect the binary search decision tree and tested SHAs
git bisect log

# Display the exact commit diff that introduced the regression
git show <CULPRIT_SHA>

# Terminate bisect session and return working tree to normal branch HEAD
git bisect reset

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When tracking down a subtle regression across hundreds of commits, `git bisect` is the gold standard. Instead of manual inspection, I write a deterministic test script that returns 0 on success and 1 on regression, and execute `git bisect run ./test.sh`. Git automatically conducts binary search, finding the exact faulty commit in less than 10 iterations. I then inspect the diff, revert or patch it, and add a regression test to the pipeline."

---

## 📌 Scenario 8: Submodule Pointer Desynchronization Breaking CI/CD Builds

### 🚨 The Production Scenario
A build pipeline fails with `fatal: reference is not a tree: <SHA>` after a developer updates a shared library Git submodule.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Git submodules track an explicit commit SHA in the parent repository, not a branch name. If a developer commits and pushes the parent repository with a new submodule pointer before pushing the corresponding commit inside the submodule's own remote repository, CI workers cloning the parent repository cannot fetch the referenced submodule SHA.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check whether the missing commit SHA exists in the submodule's remote repository.
- Step 2: Have the developer push unpushed commits from inside the submodule directory.
- Step 3: Configure Git's push safeguard: `git config push.recurseSubmodules check`.
- Step 4: Update the parent repo to reference a valid remote submodule commit.
- Step 5: Ensure CI runner clones with `git submodule update --init --recursive`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Display the commit SHAs currently checked out for each submodule
git submodule status

# Prevent pushing parent repo if submodule commits have not been pushed to remote first
git config --global push.recurseSubmodules check

# Recursively initialize and sync all submodules with their upstream remotes
git submodule update --init --recursive --remote

# Verify current local commit state within the submodule directory
cd lib/shared-utils && git log -1

# Retry pushing parent repo after ensuring submodule remote is up to date
git push origin main

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Submodule desynchronization happens because submodules record static commit SHAs in `.gitmodules`. If a developer references a local submodule commit in the parent project but forgets to push the submodule repo itself, the CI runner fails with 'reference is not a tree'. I fix this by pushing the submodule branch first, and permanently prevent it by setting `git config push.recurseSubmodules on-demand`, which forces Git to automatically push submodules before the parent."

---

## 📌 Scenario 9: Uncommitted Local Work Blocking Urgent Production Hotfix Pull

### 🚨 The Production Scenario
An on-call engineer needs to pull an urgent production hotfix branch on a bastion server, but uncommitted configuration changes block `git pull` with `error: Your local changes to the following files would be overwritten by merge`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Git safeguards against silent data loss by refusing to overwrite modified tracked files during a merge or checkout. Discarding changes with `git reset --hard` risks losing important local configuration adjustments.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Stash uncommitted changes with a descriptive label: `git stash push -u -m 'wip-bastion-config'`.
- Step 2: Pull the emergency hotfix cleanly: `git pull origin main`.
- Step 3: Execute necessary production actions.
- Step 4: Reapply the stashed work safely: `git stash pop`.
- Step 5: If conflicts arise, resolve or drop the stash cleanly.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Safely stash tracked and untracked local edits with an identifiable message
git stash push -u -m 'temporary-bastion-changes'

# Pull the latest upstream hotfix cleanly without creating merge bubbles
git pull --rebase origin main

# Inspect existing stash stack to verify saved work
git stash list

# Reapply stashed modifications and remove entry from stash stack
git stash pop

# Discard obsolete stash entries once verified
git stash drop

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Never run `git reset --hard` on production bastions unless you are 100% sure the uncommitted files are disposable. The safe approach is `git stash push -u -m 'emergency-pause'` to snapshot all tracked and untracked work. Then run `git pull --rebase` to apply the upstream hotfix cleanly, and finally `git stash pop` to re-integrate the engineer's workspace safely."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Git & Version Control Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Git & Version Control Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

