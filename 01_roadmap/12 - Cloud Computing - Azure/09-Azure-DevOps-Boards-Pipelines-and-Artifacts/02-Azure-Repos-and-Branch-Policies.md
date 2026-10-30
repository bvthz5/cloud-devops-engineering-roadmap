# 02 - Azure Repos: Git Architecture, Branch Policies & PR Governance

> **Focus:** Source control security and branch governance in Azure Repos. Deep dive into Git branching strategies (GitHub Flow vs. GitFlow), Branch Policies on `main`, automated CI Build Validation, Pull Request reviewer quorum, and Advanced Security secret scanning.

---

## 🛡️ 1. What, Why, and History of Azure Repos & Branch Policies

### What are Azure Repos and Branch Policies?
- **Azure Repos:** High-scale, cloud-hosted private Git repositories fully integrated into Azure DevOps.
- **Branch Policies:** Server-enforced governance gates applied to critical Git branches (e.g., `main`, `release/*`) that prevent direct pushes and enforce rigorous quality criteria before code can be merged via Pull Request.

### Why are Branch Policies Mandatory in Production?
1. **Prevent Direct Pushes to Production:** Accidental or malicious `git push origin main` can break running production environments immediately.
2. **Automated Quality Verification (Shift-Left):** Every Pull Request must compile cleanly, pass 100% of unit tests, and achieve code coverage standards via automated CI build triggers.
3. **Regulatory Audit Trails (SOX / SOC2):** Enterprises must mathematically prove that no line of code entered production without independent peer review from at least one authorized senior engineer.

### 📜 History & Evolution:
- **2005–2013:** Microsoft championed **TFVC (Team Foundation Version Control)**—a centralized, check-out-lock source control system.
- **2013:** Microsoft embraced Git as a first-class citizen inside Team Foundation Server.
- **2019:** Modern Branch Policies introduced with automatic PR build triggers, squash merge enforcement, and auto-complete options.
- **2023:** **GitHub Advanced Security for Azure DevOps (GHAzDO)** introduced, bringing secret scanning, push protection, and CodeQL static analysis directly into Azure Repos.

---

## 🏛️ 2. Comprehensive Branch Policy Architecture

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              PULL REQUEST GOVERNANCE PIPELINE                          │
│                                                                                        │
│   DEVELOPER BRANCH                                               PROTECTED MAIN BRANCH │
│   feature/payment-v2 ────> PULL REQUEST SUBMISSION (PR #108) ───> main                 │
│                                      │                                                 │
│                                      ▼                                                 │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ MANDATORY ENFORCED BRANCH POLICIES:                                            │   │
│   │                                                                                │   │
│   │ 1. MINIMUM NUMBER OF REVIEWERS:                                                │   │
│   │    • At least 2 approvals required.                                            │   │
│   │    • "Reset code reviewer votes when new changes are pushed" ENABLED.          │   │
│   │    • Prohibit author from approving their own PR.                              │   │
│   │                                                                                │   │
│   │ 2. BUILD VALIDATION:                                                           │   │
│   │    • Automatically triggers pipeline: 'pr-build-validation.yaml'.              │   │
│   │    • Must exit with code 0 (Unit tests & Linting pass).                        │   │
│   │                                                                                │   │
│   │ 3. WORK ITEM LINKING:                                                          │   │
│   │    • Must link to at least 1 Azure Board User Story or Bug.                    │   │
│   │                                                                                │   │
│   │ 4. STATUS CHECKS (External Gates):                                             │   │
│   │    • SonarQube Quality Gate: PASSED (Zero critical bugs).                     │   │
│   │    • Advanced Security Secret Scanner: ZERO LEAKED CREDENTIALS.               │   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
│                                      │                                                 │
│                                      ▼ All Checks Succeeded                            │
│                              SQUASH MERGE TO MAIN                                      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔄 3. Merge Strategies Compared

| Merge Strategy | Git Command | Commit History Impact | Recommendation |
| :--- | :--- | :--- | :--- |
| **Squash Merge** | `git merge --squash` | Condenses all 15 feature commits into **1 clean commit** on `main`. | **Enterprise Best Practice** (Keeps history linear) |
| **Rebase and Fast-Forward** | `git rebase` | Appends commits one by one linearly without a merge commit. | Good for small microservices |
| **Basic (No Fast-Forward)** | `git merge --no-ff` | Preserves every intermediate branch commit + creates merge commit. | Messy history; avoid in large teams |

---

## 💻 4. Practical CLI: Enforcing Branch Policies via Azure CLI

```bash
# 1. Retrieve the Repository ID
REPO_ID=$(az repos show -r "Payments-Service" --project "Payments-Modernization" --query id -o tsv)

# 2. Enforce Minimum 2 Reviewers on 'main' branch
az repos policy approver-count create \
  --repository-id $REPO_ID \
  --branch "main" \
  --minimum-approver-count 2 \
  --creator-vote-counts false \
  --reset-on-source-push true \
  --is-blocking true

# 3. Add Automated Build Validation Policy to 'main'
BUILD_DEF_ID=$(az pipelines show --name "CI-Build-Validation" --project "Payments-Modernization" --query id -o tsv)

az repos policy build create \
  --repository-id $REPO_ID \
  --branch "main" \
  --build-definition-id $BUILD_DEF_ID \
  --display-name "PR Build & Test Validation" \
  --valid-duration 720 \
  --is-blocking true

# 4. Require Linked Work Item
az repos policy work-item-linking create \
  --repository-id $REPO_ID \
  --branch "main" \
  --is-blocking true
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure DevOps Organization and Project Architecture](./01-Azure-DevOps-Organization-and-Project-Architecture.md) | [Index](../../../README.md) | [03 - Azure Pipelines YAML Pipeline Architecture and Syntax →](./03-Azure-Pipelines-YAML-Pipeline-Architecture-and-Syntax.md) |
