# 04 - IaC Workflow: Write → Plan → Apply

## 1. The Universal IaC Lifecycle

```text
┌──────────────────────────────────────────────────────────────────┐
│              INFRASTRUCTURE AS CODE LIFECYCLE                     │
│                                                                   │
│  ┌───────┐   ┌──────────┐   ┌───────┐   ┌───────┐   ┌────────┐ │
│  │ WRITE │──►│ VALIDATE │──►│ PLAN  │──►│ APPLY │──►│DESTROY │ │
│  │ .tf   │   │ fmt/lint │   │ diff  │   │execute│   │teardown│ │
│  └───────┘   └──────────┘   └───────┘   └───────┘   └────────┘ │
│      │            │              │            │            │      │
│      ▼            ▼              ▼            ▼            ▼      │
│   Git push    terraform      terraform    terraform   terraform  │
│   PR review   validate       plan         apply       destroy    │
│               terraform fmt  (saved plan)  -auto-approve          │
└──────────────────────────────────────────────────────────────────┘
```

## 2. Phase-by-Phase Breakdown

### Phase 1: Write
```bash
# Initialize project structure
mkdir -p infra/{modules,environments/{dev,staging,prod}}

# Key files
# main.tf       — Resource definitions
# variables.tf  — Input variable declarations
# outputs.tf    — Output value declarations
# providers.tf  — Provider configuration & versions
# terraform.tf  — Backend configuration & required providers
# versions.tf   — Version constraints
```

### Phase 2: Validate & Format
```bash
# Format all .tf files to canonical style
terraform fmt -recursive

# Validate syntax and internal consistency
terraform validate

# Run static analysis (tflint)
tflint --init && tflint
```

### Phase 3: Plan (Preview)
```bash
# Generate execution plan
terraform plan -out=tfplan

# The plan shows:
# + resource to CREATE
# ~ resource to UPDATE (in-place)
# -/+ resource to REPLACE (destroy + recreate)
# - resource to DESTROY
```

### Phase 4: Apply (Execute)
```bash
# Apply the saved plan (safest approach)
terraform apply tfplan

# Or interactive apply (prompts for yes/no)
terraform apply

# Auto-approve (CI/CD pipelines only)
terraform apply -auto-approve
```

### Phase 5: Destroy (Teardown)
```bash
# Preview what will be destroyed
terraform plan -destroy

# Execute destruction
terraform destroy
```

## 3. The Saved Plan Pattern (Production Best Practice)

```text
CI Pipeline:
  terraform plan -out=tfplan    →  Human reviews plan output
  terraform apply tfplan         →  Executes EXACTLY what was reviewed
                                    (not what the state looks like NOW)
```

**Why?** Between `plan` and `apply`, someone might change resources manually. A saved plan locks the exact set of changes that were reviewed.

## 4. Version Controlling Infrastructure

```text
.
├── .gitignore           # Ignore .terraform/, *.tfstate, *.tfplan
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
├── terraform.tf
└── environments/
    ├── dev.tfvars
    ├── staging.tfvars
    └── prod.tfvars
```

### Critical `.gitignore` for Terraform
```gitignore
.terraform/
*.tfstate
*.tfstate.backup
*.tfplan
.terraform.lock.hcl   # Commit this for reproducibility
crash.log
override.tf
override.tf.json
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Mutable vs Immutable](./03-Mutable-vs-Immutable-Infrastructure.md) | [README](./README.md) | [05 - IaC in SDLC](./05-IaC-in-the-SDLC-and-DevOps-Pipeline.md) |
