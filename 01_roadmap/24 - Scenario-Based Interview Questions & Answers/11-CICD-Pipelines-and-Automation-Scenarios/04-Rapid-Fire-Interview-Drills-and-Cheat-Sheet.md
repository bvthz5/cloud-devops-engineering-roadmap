# CI/CD Pipelines & Automation Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Git Branch Protection Bypass Leads to Broken Main Build and Release Outage

### 🚨 The Production Scenario
An engineer with Administrator privileges directly pushed an untested feature branch directly to the 'main' branch, breaking production compilation and blocking subsequent hotfixes during an active production outage.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
GitHub repository branch protection rules lacked 'Include administrators' enforcement, and required status checks and linear history were not mandated.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately revert the rogue commit on 'main' using 'git revert -m 1' or force-push recovery if agreed upon.
- Update Branch Protection Rules / Repository Rulesets to enforce 'Do not allow bypassing the above settings' for all users including Org Admins.
- Require minimum 2 peer code review approvals, code owner approvals (CODEOWNERS), and passing CI status checks before merge.
- Enforce linear commit history or squash merging to ensure bisectable Git history.
- Establish an Emergency Bypass workflow requiring dual-authorization audit logging for break-glass scenarios.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify rogue commit on main branch
git log -n 3 --oneline

# Revert breaking merge commit cleanly on main
git revert -m 1 <ROGUE_MERGE_COMMIT_HASH> --no-edit && git push origin main

# Create enterprise repository ruleset via GitHub CLI
gh ruleset create --repo owner/repo --name 'Strict-Main-Protection' --enforcement active

# Update branch protection policy enforcing admin restrictions
gh api repos/owner/repo/branches/main/protection --input protection-rules.json -X PUT

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Even administrators must never push directly to production branches. I enforce modern GitHub Repository Rulesets with 'Do not allow bypassing' applied universally, including organization owners. Merges require green CI checks, CODEOWNERS approval, and linear history. For true emergencies, break-glass access requires a documented issue ticket and generates real-time Slack/PagerDuty audit alerts."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: GitHub Actions Self-Hosted Runners Swamped by Job Concurrency Bottlenecks** | `kubectl get runnerdeployments -n arc-systems` | Runners were statically provisioned without ephemeral auto-scaling. Builds did n... |
| **Scenario 2: Jenkins Zombie Builds & Master Controller Heap OOM Crash** | `jcmd <JENKINS_PID> GC.heap_info` | Jenkins was running heavy build workloads and pipeline scripts directly on the c... |
| **Scenario 3: GitLab CI Pipeline Failure Due to Secret Masking and Regex Collision** | `glab variable list --repo owner/repo` | A masked secret variable in GitLab CI was set to a common short string (e.g., '1... |
| **Scenario 4: Dependency Vulnerability Scan (Trivy/Snyk) Fails CI Build on Zero-Day CVE** | `trivy image --severity CRITICAL --ignore-unfixed my-app:latest` | The CI pipeline enforces a strict security gate: 'exit-code 1 if severity == CRI... |
| **Scenario 5: Multi-Architecture Docker Builds (amd64 / arm64) Slow Pipeline to 45 Minutes** | `docker buildx create --name native-builder --driver docker-container --platform linux/amd64` | QEMU user-mode emulation translates CPU instructions in software, incurring an 8... |
| **Scenario 6: Pipeline Supply Chain Attack - NPM Dependency Typosquatting** | `npm ci --ignore-scripts` | The project lacked a package-lock.json integrity check, and the CI environment p... |
| **Scenario 7: Database Migration Job in CI/CD Causes Production Table Lock & Downtime** | `SELECT pid, query, state, age(clock_timestamp(), query_start) FROM pg_stat_activity WHERE state != 'idle' ORDER BY age DESC;` | The migration script ran 'ALTER TABLE orders ADD COLUMN status VARCHAR(20) DEFAU... |
| **Scenario 8: Flaky Integration Tests Stall Pull Request Velocity by 30%** | `pytest --durations=10 --maxfail=3` | Tests relied on hardcoded sleep timers (`time.sleep(5)`) rather than explicit ev... |
| **Scenario 9: CI Pipeline Fails with 'Docker Hub Pull Rate Limit Exceeded (HTTP 429)'** | `crane copy docker.io/library/alpine:3.19 internal-registry.corp/library/alpine:3.19` | CI worker nodes were pulling public base images directly from Docker Hub without... |
| **Scenario 10: Git Branch Protection Bypass Leads to Broken Main Build and Release Outage** | `git log -n 3 --oneline` | GitHub repository branch protection rules lacked 'Include administrators' enforc... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← CI/CD Pipelines & Automation Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [GitOps & Continuous Delivery Scenarios: Production Incidents & Triage Scenarios →](../12-GitOps-and-Continuous-Delivery-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

