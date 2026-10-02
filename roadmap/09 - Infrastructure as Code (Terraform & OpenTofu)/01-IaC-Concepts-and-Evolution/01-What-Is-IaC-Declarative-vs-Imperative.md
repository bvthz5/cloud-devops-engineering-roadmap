# 01 - What Is IaC: Declarative vs Imperative

## 1. Defining Infrastructure as Code (IaC)

Infrastructure as Code is the practice of managing and provisioning computing infrastructure through machine-readable definition files rather than through interactive configuration tools or physical hardware configuration.

```text
┌────────────────────────────────────────────────────────────────────┐
│                  TRADITIONAL vs IaC APPROACH                       │
│                                                                    │
│  BEFORE IaC (Manual / ClickOps)         WITH IaC (Code-Driven)    │
│  ─────────────────────────────          ──────────────────────     │
│  ✗ SSH into servers                     ✓ Write .tf / .yaml       │
│  ✗ Run ad-hoc shell commands            ✓ Version control (Git)   │
│  ✗ Click through cloud consoles         ✓ terraform plan (preview)│
│  ✗ No audit trail                       ✓ terraform apply (deploy)│
│  ✗ Configuration drift inevitable       ✓ Idempotent operations   │
│  ✗ "Works on my machine"               ✓ Reproducible everywhere │
└────────────────────────────────────────────────────────────────────┘
```

## 2. Declarative vs Imperative Paradigms

| Aspect | Declarative | Imperative |
|---|---|---|
| **Definition** | You describe the **desired end state**; the tool determines the steps | You describe the **exact sequence of steps** to reach the state |
| **Example Tool** | Terraform, CloudFormation, Ansible (YAML) | Bash scripts, AWS CLI, Pulumi (general-purpose code) |
| **Idempotency** | Built-in — applying the same config twice produces no change | Must be explicitly handled by the developer |
| **Dependency Mgmt** | Implicit via resource graph | Manual ordering required |
| **Drift Detection** | `terraform plan` shows delta | Must write custom diff scripts |
| **Learning Curve** | Domain-specific (HCL, YAML) | General-purpose programming |

### Declarative Example (Terraform HCL)
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  tags = {
    Name = "web-server"
  }
}
```

### Imperative Example (AWS CLI / Bash)
```bash
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --instance-type t3.micro \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=web-server}]'
```

## 3. Idempotency — The Core Principle

**Idempotent operation:** Applying the same operation multiple times produces the same result as applying it once.

```text
terraform apply (1st run)  →  Creates 1 EC2 instance
terraform apply (2nd run)  →  "No changes. Infrastructure is up-to-date."
terraform apply (3rd run)  →  "No changes. Infrastructure is up-to-date."
```

Without idempotency, the imperative script would create 3 instances.

## 4. Key IaC Principles

1. **Version Control** — All infrastructure code lives in Git; every change has a commit hash, author, and review trail
2. **Code Review** — Infrastructure changes go through PR/MR review before `apply`
3. **Reproducibility** — The same code produces the same infrastructure in dev, staging, and production
4. **Self-Documenting** — The `.tf` files ARE the documentation of your infrastructure
5. **Blast Radius Control** — Small, modular changes reduce the scope of potential failures

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - IaC Tooling Landscape](./02-IaC-Tooling-Landscape-Terraform-Pulumi-CloudFormation.md) |
