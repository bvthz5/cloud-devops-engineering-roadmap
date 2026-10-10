# 04 - Atlantis: PR-Based Terraform & OpenTofu Automation

Atlantis is an open-source, self-hosted GitOps server that executes Terraform and OpenTofu workflows entirely through Pull Request (PR) comments. By moving execution into PRs, Atlantis enables collaborative peer review of infrastructure changes, prevents state locking collisions between developers, and ensures that code merged to `main` accurately reflects running cloud infrastructure.

---

## 1. The Core Paradigm: Apply-on-Merge vs. Apply-in-PR

In traditional software development, CI/CD pipelines compile and test on pull requests, but deploy to production only *after* the code merges into `main`. Applying this model to Infrastructure as Code leads to severe operational failures:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                 TRADITIONAL CI/CD RUNNER VS. ATLANTIS GITOPS                     │
│                                                                                 │
│   ❌ TRADITIONAL CI (Apply on Merge to main)    ✅ ATLANTIS (Apply in PR)       │
│                                                                                 │
│   1. Engineer opens PR (plan succeeds)          1. Engineer opens PR            │
│   2. PR merged into main branch                 2. Atlantis comments plan output│
│   3. CI runner starts `terraform apply` on main 3. SRE team reviews & approves  │
│   4. Cloud API throws 409 Conflict:             4. Engineer comments:           │
│      CIDR block overlaps or quota exceeded!        "atlantis apply"             │
│   5. RESULT: `main` branch is BROKEN!           5. Cloud provisions & succeeds! │
│      Git says resource exists, cloud disagrees. 6. Atlantis auto-merges to main.│
│      Next developer cannot deploy anything!     7. RESULT: `main` is ALWAYS     │
│                                                    in sync with the cloud!      │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The Atlantis Pull Request Lifecycle & Locking Architecture

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                         ATLANTIS LIFECYCLE SEQUENCE                             │
│                                                                                 │
│   DEVELOPER                 GIT PLATFORM (GitHub)               ATLANTIS SERVER │
│       │                              │                                 │        │
│   1.  │──── Git Push Feature Branch ─►│                                 │        │
│       │                              │── Webhook: PR Opened ──────────►│        │
│       │                              │                                 │        │
│   2.  │                              │                        Acquires Repo Lock│
│       │                              │                        Runs terraform plan
│       │                              │◄── Comments Plan Output ────────┤        │
│   3.  │◄── Reviews Plan Output ──────│                                 │        │
│       │                              │                                 │        │
│   4.  │── Approves PR (Peer Review) ─►│                                 │        │
│       │                              │                                 │        │
│   5.  │── Comments: "atlantis apply"─►│                                 │        │
│       │                              │── Webhook: Comment Event ──────►│        │
│       │                              │                                 │        │
│   6.  │                              │                        Validates Approvals
│       │                              │                        Runs terraform apply
│       │                              │◄── Comments Apply Result ───────┤        │
│   7.  │                              │◄── Auto-Merges PR & Closes Lock ┤        │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The Atlantis Distributed Locking Engine (`atlantis lock`)
When an engineer opens a PR affecting `environments/prod/vpc`, Atlantis places an internal lock on that specific directory and workspace. 
- If a second engineer opens a conflicting PR modifying the same directory, Atlantis **rejects** the second plan with a comment:
  > *"This project is currently locked by PR #142 opened by @alice. Wait until PR #142 is merged or unlocked."*
- This mechanism completely eliminates race conditions where two developers plan simultaneously against stale state files.

---

## 3. Production Configuration: `atlantis.yaml`

The repo-level `atlantis.yaml` file defines directory mappings, workspaces, custom execution stages, and security guardrails.

```yaml
version: 3

# Enforce strict merge prerequisites
automerge: true
delete_source_branch_on_merge: true

projects:
  - name: networking-production
    dir: terraform/production/networking
    workspace: default
    terraform_version: 1.6.5
    autoplan:
      when_modified:
        - "*.tf"
        - "*.tfvars"
        - "../../modules/vpc/**/*.tf"
      enabled: true
    apply_requirements:
      - approved     # Requires peer review approval before apply can run
      - mergeable    # Branch must be up-to-date with main and have no merge conflicts
    workflow: hardened-security-workflow

workflows:
  hardened-security-workflow:
    plan:
      steps:
        - run: tfsec . --format=checkstyle > /tmp/tfsec.xml || true
        - run: trivy config . --severity HIGH,CRITICAL --exit-code 1
        - init
        - plan:
            extra_args: ["-lock-timeout=15m"]
    apply:
      steps:
        - apply
        - run: |
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"✅ Terraform Apply successfully completed on production networking!"}' \
            $SLACK_WEBHOOK_URL
```

---

## 4. Deploying Atlantis on Kubernetes (Production Deployment)

Atlantis is typically deployed as a lightweight Kubernetes Deployment inside a private management cluster with access to cloud provider APIs.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: atlantis
  namespace: platform-tools
spec:
  replicas: 1 # Atlantis requires stateful file locks on disk or Redis; scale to 1 or use persistent disk
  selector:
    matchLabels:
      app: atlantis
  template:
    metadata:
      labels:
        app: atlantis
    spec:
      serviceAccountName: atlantis-aws-iam-role # Bound to AWS IAM OIDC Workload Identity
      containers:
        - name: atlantis
          image: ghcr.io/runatlantis/atlantis:v0.27.0
          env:
            - name: ATLANTIS_GH_USER
              value: "enterprise-bot"
            - name: ATLANTIS_GH_TOKEN
              valueFrom:
                secretKeyRef:
                  name: atlantis-secrets
                  key: github-token
            - name: ATLANTIS_GH_WEBHOOK_SECRET
              valueFrom:
                secretKeyRef:
                  name: atlantis-secrets
                  key: webhook-secret
            - name: ATLANTIS_REPO_ALLOWLIST
              value: "github.com/my-org/*"
            - name: ATLANTIS_DEFAULT_TF_VERSION
              value: "1.6.5"
            - name: ATLANTIS_PORT
              value: "4141"
          ports:
            - containerPort: 4141
          resources:
            requests:
              cpu: "500m"
              memory: "1Gi"
            limits:
              cpu: "2"
              memory: "4Gi"
```

---

## 5. Security Controls & Enterprise Guardrails

1. **Strict Webhook Authentication:** Atlantis must validate GitHub/GitLab cryptographic signatures (`X-Hub-Signature-256`) using a shared secret before processing any webhook event.
2. **Eliminate Static Credentials via OIDC:** Run Atlantis with native cloud workload identity (AWS IAM Roles for Service Accounts - IRSA, GCP Workload Identity, or Azure Pod Identity). Never mount permanent cloud access keys on disk.
3. **Repository Allowlisting (`ATLANTIS_REPO_ALLOWLIST`):** Explicitly restrict which repositories Atlantis will respond to, preventing rogue repositories from stealing cluster privileges.
4. **Disallow Custom Server Arguments in PRs:** By default, Atlantis disables the `--allow-custom-workflows` flag on untrusted repos to prevent malicious pull requests from injecting arbitrary Bash commands into `atlantis.yaml`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - GitLab CI for Terraform](./03-GitLab-CI-for-Terraform.md) | [Index](../../../README.md) | [05 - Jenkins Terraform Pipeline →](./05-Jenkins-Terraform-Pipeline.md) |

