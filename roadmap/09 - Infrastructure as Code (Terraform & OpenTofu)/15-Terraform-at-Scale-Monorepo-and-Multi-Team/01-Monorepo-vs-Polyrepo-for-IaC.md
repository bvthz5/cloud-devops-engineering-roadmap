# 01 - Monorepo vs. Polyrepo for Enterprise Infrastructure as Code

As an organization scales beyond a handful of engineers, deciding how to structure Infrastructure as Code repositories directly impacts security boundaries, blast radius, release velocity, and developer autonomy. Infrastructure platform architects must weigh the trade-offs between housing all infrastructure definitions in a single **Monorepo** versus distributing code across dozens of independent **Polyrepos**.

---

## 1. Architectural Comparison: Monorepo vs. Polyrepo

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       MONOREPO VS. POLYREPO ARCHITECTURE                        │
│                                                                                 │
│   ENTERPRISE MONOREPO                           POLYREPO ECOSYSTEM              │
│   (Single Centralized Git Tree)                 (Decentralized Service Repos)   │
│                                                                                 │
│   github.com/org/infrastructure                 github.com/org/vpc-infra        │
│   ├── modules/                                  ├── main.tf                     │
│   │   ├── vpc/                                  └── terraform.tfstate           │
│   │   ├── eks/                                                                  │
│   │   └── rds/                                  github.com/org/payments-infra   │
│   └── environments/                             ├── main.tf                     │
│       ├── dev/                                  └── terraform.tfstate           │
│       ├── staging/                                                              │
│       └── prod/                                 github.com/org/auth-infra       │
│                                                 ├── main.tf                     │
│                                                 └── terraform.tfstate           │
│   • Single atomic pull request                  • Isolated blast radius         │
│   • Global visibility & linting                 • Independent team autonomy     │
│   • Complex CI/CD path filtering                • High module version drift     │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Deep-Dive: Enterprise Monorepo Pattern

In a monorepo, all cloud infrastructure code, reusable module definitions, and environment instantiations live inside a single version-controlled repository.

### Advantages
1. **Single Source of Truth & Universal Search:** Engineers can inspect the entire organization's cloud topology in one repository. Finding which security group opens port 443 or which subnet hosts Redis takes seconds using native grep or GitHub search.
2. **Atomic Cross-Cutting Refactoring:** If the platform team adds a mandatory tag or updates a VPC module, they can submit a single pull request that updates the module *and* tests the change across all environments simultaneously.
3. **Unified Security & Compliance Policies:** Tools like Trivy, Checkov, OPA, and TFLint are configured once in the root `.tflint.hcl` and applied consistently across every environment.

### Operational Challenges
1. **CI/CD Path-Filter Complexity:** CI runners must use intelligent path filtering (e.g., `dorny/paths-filter` in GitHub Actions) to only trigger plans for folders that actually changed. Without path filtering, a one-line comment change in `dev/` triggers plans across 50 production workspaces.
2. **Merge Queue & Git Contention:** High commit volume from multiple teams can lead to frequent rebase friction and merge queue bottlenecks.
3. **Access Control Friction:** Enforcing granular permissions requires strict `CODEOWNERS` configurations to prevent product developers from accidentally approving changes to the core networking layer.

```text
# Sample .github/CODEOWNERS configuration in an IaC Monorepo
*                               @org/cloud-platform-leads
/modules/networking/            @org/network-engineers
/environments/production/       @org/sre-oncall @org/security-reviewers
/environments/dev/              @org/product-developers
```

---

## 3. Deep-Dive: Polyrepo Pattern

In a polyrepo model, infrastructure code is split across many repositories—either organized by lifecycle layer (e.g., `repo-networking`, `repo-security`, `repo-compute`) or embedded directly alongside product microservice applications.

### Advantages
1. **Total Team Autonomy:** Product teams manage their own infrastructure repository without waiting in shared queues or needing reviews from unrelated teams.
2. **Strict Blast Radius & IAM Boundary Isolation:** CI/CD runners for a payments repository only hold AWS IAM credentials for the Payments AWS account. Compromising a polyrepo runner cannot compromise the core organization networking root.
3. **Simple, Predictable CI/CD:** Every commit to the repository triggers a plan for that specific service without complex path-matching logic.

### Operational Challenges
1. **The "Module Version Drift" Nightmare:** Shared modules are consumed via Git tags (e.g., `source = "git::https://github.com/org/tf-modules.git//vpc?ref=v2.1.0"`). Teams rarely update their versions voluntarily, resulting in different services running 15 conflicting versions of the same module with unpatched security vulnerabilities.
2. **High Auditing & Governance Overhead:** Rolling out a company-wide security rule (e.g., enforcing IMDSv2 across all EC2 instances) requires opening, tracking, and merging 80 individual pull requests across 80 different repositories.

---

## 4. Comprehensive Comparison Matrix

| Operational Dimension | Monorepo Pattern | Polyrepo Pattern |
| :--- | :--- | :--- |
| **Code Discovery & Visibility** | **Trivial** (Single clone gives full cloud view) | Fragmented (Requires crawling dozens of repos) |
| **Blast Radius of a Bad PR** | Moderate (Mitigated via directory state splitting) | **Minimal** (Confined strictly to that repo) |
| **IAM Credential Scope in CI** | Requires dynamic OIDC role assumption per path | **Strictly partitioned** per repository |
| **Module Upgrades** | Instant updates across all consumer directories | Requires updating Git tags in every repo |
| **CI/CD Configuration** | Requires advanced path-based trigger filtering | Standard per-repo triggers |
| **Governance & Policy as Code** | Enforced centrally at root repository level | Requires organization-level GitHub rulesets |
| **Ideal Organizational Fit** | Platform engineering teams, startups to mid-scale | Large enterprises with 50+ decentralized teams |

---

## 5. The Industry Best Practice: The Hybrid Platform Model

Top-tier technology organizations adopt a **Hybrid Platform Architecture**:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                     THE HYBRID ENTERPRISE PLATFORM PATTERN                      │
│                                                                                 │
│   1. CENTRAL MODULE CATALOG (Monorepo)         2. LIVE DEPLOYMENTS (Terragrunt) │
│      github.com/org/terraform-modules             github.com/org/cloud-live     │
│      ├── vpc/                                     ├── production/               │
│      ├── eks-cluster/                             │   ├── us-east-1/            │
│      └── aurora-pg/                               │   │   ├── networking/       │
│      (Maintained by Platform Team;                │   │   └── data-tier/        │
│       Semantic Version Releases)                  │   └── eu-west-1/            │
│                                                   └── staging/                  │
│                                                   (Monorepo for orchestrating   │
│                                                    live multi-account states)   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

1. **Centralized Modules Monorepo (`terraform-modules`):** Platform engineers publish hardened, reusable, CIS-compliant modules with semantic versions (`v1.4.0`).
2. **Centralized Live Environments Monorepo (`cloud-live`):** Teams instantiate those modules using Terragrunt or directory-based Terraform, enjoying single-pane-of-glass visibility while maintaining isolated remote state backends.
3. **Embedded Service Infra for Microservices:** Pure application-tier resources (e.g., S3 upload buckets, SQS queues) can live inside the microservice application repository, consuming outputs exported by the central network.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (14-CICD-for-Terraform-Atlantis-and-Pipelines)](../14-CICD-for-Terraform-Atlantis-and-Pipelines/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Multi Team Collaboration Patterns →](./02-Multi-Team-Collaboration-Patterns.md) |

