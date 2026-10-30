# 01 - IaC Security Threat Model & Attack Surface

## 1. Why Infrastructure as Code Is the Highest-Value Cyber Attack Surface

In traditional enterprise security, attackers targeted web application vulnerabilities (such as SQL injections or cross-site scripting) to breach individual databases. In the cloud-native era, **Infrastructure as Code (IaC) has become the single highest-value target for threat actors**.

When an organization manages its cloud infrastructure via Terraform, OpenTofu, or CloudFormation:
1. The IaC pipeline possesses **God-mode privileges** (often `AdministratorAccess` across entire AWS, Azure, or GCP organizations).
2. The `terraform.tfstate` database holds every plaintext secret, database master password, and private key used across the enterprise.
3. A single misconfiguration in an HCL manifest (such as `cidr_blocks = ["0.0.0.0/0"]` on an RDS database) propagates instantly across thousands of cloud instances in seconds.

A breach of the IaC pipeline does not compromise a single application; it grants total administrative takeover of the company's entire global infrastructure estate.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE INFRASTRUCTURE AS CODE ATTACK SURFACE TAXONOMY                         |
+---------------------------------------------------------------------------------------------------+

                               ┌───────────────────────────┐
                               │  ATTACKER GOAL: FULL      │
                               │  CLOUD INFRASTRUCTURE     │
                               │  TAKEOVER                 │
                               └─────────────┬─────────────┘
                                             │
         ┌───────────────────────────────────┼───────────────────────────────────┐
         ▼                                   ▼                                   ▼
 1. SECRETS EXFILTRATION             2. SUPPLY CHAIN POISONING           3. MISCONFIGURATION INJECTION
 - State file stolen from S3         - Malicious third-party provider    - Public S3 bucket created
 - Hardcoded credentials in Git       downloaded from public registry    - Security Group opens port 22
 - Plaintext DB master passwords     - Compromised GitHub Action runner  - Unencrypted EBS volumes
```

---

## 2. The 5 Major IaC Attack Vectors in Depth

### Vector 1: Plaintext Secrets in State Files and Git Repositories
The #1 source of enterprise cloud credential compromise is the accidental commitment of sensitive variables.
- Developers hardcode passwords in `terraform.tfvars` and accidentally push to GitHub. Even if the commit is deleted, the secret remains in the Git reflog history permanently.
- Furthermore, because Terraform stores every resource attribute as **plaintext JSON inside `terraform.tfstate`**, an attacker who gains read access to the state S3 bucket instantly extracts all database master passwords, API tokens, and TLS private keys.

### Vector 2: Over-Privileged CI/CD IAM Execution Roles
CI/CD runners (such as GitHub Actions or GitLab CI) are often lazily granted AWS `AdministratorAccess`. 
If an attacker submits a Pull Request containing a malicious test script or compromises an NPM dependency in the CI pipeline, they can execute arbitrary shell commands inside the runner, assume the administrator IAM role, and exfiltrate cloud credentials.

### Vector 3: Supply Chain Attacks via Unsigned Provider Plugins
Because Terraform dynamically downloads provider binaries during `terraform init`, attackers attempt **typosquatting attacks** on public registries (e.g., publishing a malicious binary named `hashicorp-aws-provider` instead of `hashicorp/aws`). Because providers run as arbitrary local OS child processes, a malicious provider can execute reverse shells, install cryptominers, or steal SSH keys directly from developer workstations.

### Vector 4: Permissive Security Defaults
Cloud providers often have permissive defaults. If an engineer creates an `aws_security_group` without explicit egress restrictions, AWS allows outbound traffic to the entire internet (`0.0.0.0/0`). If an S3 bucket is provisioned without explicit public blocks, malicious actors can scan for bucket enumeration.

---

## 3. The "Shift-Left" Defense-in-Depth Pipeline

To secure Infrastructure as Code, enterprise security teams implement a **Shift-Left Defense-in-Depth Pipeline**. Security checks are enforced at every stage of the software development lifecycle before infrastructure is ever created:

```text
+---------------------------------------------------------------------------------------------------+
|                        THE SHIFT-LEFT IaC SECURITY ENFORCEMENT PIPELINE                           |
+---------------------------------------------------------------------------------------------------+

 1. DEVELOPER WORKSTATION (Pre-Commit Gate)
    Tools: pre-commit hooks, detect-secrets, git-secrets, tflint
    Action: Blocks commit locally if high-entropy passwords or syntax errors are detected!
          │
          ▼
 2. PULL REQUEST / CI BUILD (Static Analysis & SAST Gate)
    Tools: tfsec, Checkov, Trivy
    Action: Scans HCL code against CIS Benchmarks (flags unencrypted disks, open security groups).
          │
          ▼
 3. EXECUTION PLAN AUDIT (Policy-as-Code Gate)
    Tools: Open Policy Agent (OPA Rego), Conftest, HashiCorp Sentinel
    Action: Analyzes "terraform plan -json". Fails the build if plan violates compliance rules
            (e.g., "All EC2 instances must have an 'Owner' and 'Environment' tag").
          │
          ▼
 4. APPLY GATE (Least-Privilege OIDC Execution)
    Tools: AWS IAM OIDC Workload Identity (Zero static AWS keys stored in CI secrets!).
    Action: Provisions infrastructure using a tightly scoped, temporary assumed role.
          │
          ▼
 5. RUNTIME DRIFT & CSPM MONITORING (Post-Deployment Gate)
    Tools: AWS Config, CloudTrail, Wiz, Prisma Cloud
    Action: Continuously monitors production for unauthorized console modifications.
```

---

## 4. Eliminating Long-Lived Credentials via OIDC Workload Identity

A critical security imperative is **never storing static AWS Access Keys (`AKIA...`) inside GitHub Actions or GitLab CI Secrets**. If secrets are compromised, attackers retain permanent access.

Modern IaC pipelines use **OpenID Connect (OIDC) Workload Identity Federation**:
1. When a GitHub Actions workflow executes, GitHub generates a short-lived, cryptographically signed OIDC JWT token.
2. The runner presents this JWT to the AWS Security Token Service (`sts:AssumeRoleWithWebIdentity`).
3. AWS verifies the signature against GitHub's public OIDC keys, confirms the repository and branch name match an IAM trust policy condition, and issues a **temporary security token valid for 1 hour**.

```hcl
# AWS IAM Trust Policy for GitHub Actions OIDC:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789012:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
        },
        "StringLike": {
          "token.actions.githubusercontent.com:sub": "repo:enterprise/infrastructure-live:ref:refs/heads/main"
        }
      }
    }
  ]
}
```

---

## 5. Security Checklist for Production IaC Repositories

| Security Domain | Mandatory Production Control | Enforcement Mechanism |
| :--- | :--- | :--- |
| **Secrets in Git** | Zero static passwords or keys in `.tf` or `.tfvars`. | `pre-commit` hook with `detect-secrets`. |
| **State File Protection** | Remote S3 backend encrypted with Customer KMS Key + Block Public Access. | IAM Bucket Policy + KMS Key Policy. |
| **State File Access** | Restrict state read access exclusively to CI/CD and break-glass admins. | Least-privilege IAM Roles. |
| **Static Code Analysis** | Automated SAST scanning on every Pull Request. | `tfsec` and `checkov` in CI pipeline. |
| **Dependency Integrity** | Mandatory commit of `.terraform.lock.hcl` with GPG signature checks. | CI pipeline failure if lock file is modified. |
| **CI/CD Authentication** | OIDC Workload Identity Federation (Zero static cloud credentials). | AWS STS `AssumeRoleWithWebIdentity`. |
| **Destructive Safeguards** | Enforce `prevent_destroy = true` lifecycle on production databases and VPCs. | HCL `lifecycle` meta-arguments. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (09-Testing-Terraform-and-Drift-Detection)](../09-Testing-Terraform-and-Drift-Detection/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - tfsec Static Security Scanner →](./02-tfsec-Static-Security-Scanner.md) |
