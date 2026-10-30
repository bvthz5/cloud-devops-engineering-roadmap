# 03 - Declarative `removed` Blocks & Low-Level State Surgery

In enterprise Infrastructure as Code lifecycles, migrating infrastructure between tools, decommissioning legacy systems, or offloading resources to dedicated platform teams requires safely unmanaging resources without destroying them in the cloud. Historically, this required error-prone manual CLI commands (`terraform state rm`). Starting in **Terraform 1.7.0** (and OpenTofu), the declarative `removed` block enables version-controlled, PR-reviewed state pruning and resource retirement.

---

## 1. The Production Crisis: Deletion vs. De-Registration

In standard Terraform execution, removing a `resource` block from code signals to the engine that the physical cloud object is no longer desired:

$$\Delta = \text{Desired State (Empty)} - \text{Current State (Running VM)} \implies \text{Action: DESTROY}$$

If an engineer deletes an Amazon RDS Aurora cluster block from `main.tf` because another team is taking over management, running `terraform apply` will issue a catastrophic `DeleteDBCluster` API call, wiping out terabytes of production data!

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    RESOURCE RETIREMENT: THREE BEHAVIORAL PATHS                  │
│                                                                                 │
│   1. NATIVE CODE DELETION         2. MANUAL CLI SURGERY        3. DECLARATIVE   │
│      (Delete resource block)         (terraform state rm)         REMOVED BLOCK │
│                                                                                 │
│   ┌─────────────────────┐         ┌─────────────────────┐   ┌─────────────────┐ │
│   │ Delete block in .tf │         │ Run CLI command on  │   │ removed {       │ │
│   └──────────┬──────────┘         │ local terminal      │   │   from = ...    │ │
│              │                    └──────────┬──────────┘   │   destroy=false │ │
│              ▼                               ▼              │ }               │ │
│   ┌─────────────────────┐         ┌─────────────────────┐   └────────┬────────┘ │
│   │ terraform apply     │         │ Bypasses Git PRs    │            │          │
│   │ DESTROYS cloud      │         │ Race condition in CI│            ▼          │
│   │ infrastructure! 💥  │         │ Risk of human error │   ┌─────────────────┐ │
│   └─────────────────────┘         └─────────────────────┘   │ Reviewed in Git │ │
│                                                             │ No destruction! │ │
│                                                             │ State unmanaged │ │
│                                                             └─────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The Declarative `removed` Block (Terraform >= 1.7 & OpenTofu)

The `removed` block informs Terraform that a resource previously managed in state has been removed from configuration, and dictates whether the physical cloud resource should be destroyed or retained.

### Syntax & Lifecycle Configuration

```hcl
# Scenario A: Stop managing the RDS cluster in Terraform, but KEEP IT RUNNING in AWS!
removed {
  from = aws_db_instance.primary_database

  lifecycle {
    destroy = false   # Cloud API DELETE is NOT executed; resource is unmanaged
  }
}

# Scenario B: Explicitly document intent to destroy a deprecated Redis cache
removed {
  from = aws_elasticache_cluster.legacy_cache

  lifecycle {
    destroy = true    # Standard behavior: physical resource will be terminated
  }
}
```

### Why Declarative `removed` Beats `terraform state rm` in Teams
1. **Pull Request Code Review:** Teammates and security leads can verify that `destroy = false` before the state is modified.
2. **CI/CD Pipeline Compatibility:** Eliminates the need for engineers to request emergency AWS IAM write credentials or SSH access to run manual terminal commands against production state.
3. **Reproducibility Across Workspaces:** A team managing `dev`, `staging`, and `production` workspaces can merge a single Git commit; the resource will be cleanly unmanaged across all three environments as each pipeline executes.

---

## 3. Low-Level State Surgery Commands & Mechanics

When refactoring complex modules or recovering from corruption, engineers must utilize low-level state surgery tools.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           STATE SURGERY TOOLKIT                                 │
│                                                                                 │
│   COMMAND             PURPOSE                                STATE IMPACT       │
│   ─────────────────── ────────────────────────────────────── ─────────────────── │
│   state list          Enumerate all resource addresses       Read-Only          │
│   state show <addr>   Dump full attribute schema of one item Read-Only          │
│   state mv <src> <dst>Rename address or migrate across files Moves JSON Object  │
│   state rm <addr>     Orphan resource from tracking          Deletes from State │
│   state pull          Fetch raw JSON to stdout               Backup Snapshot    │
│   state push <file>   Force write modified state to backend  Serial + 1 Update  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 1️⃣ `terraform state rm` (Orphaning Resources Manually)

Used to excise a resource from state without sending any deletion API requests to the cloud provider.

```bash
# Orphan a single EC2 instance
terraform state rm aws_instance.monitored_node

# Orphan an entire child module and all of its nested child resources
terraform state rm module.vpc.aws_subnet.public
```

> [!CAUTION]
> Once removed, Terraform loses all knowledge of the resource. Future changes in code will attempt to re-create the resource, causing name collision errors (e.g., `BucketAlreadyExists` or `VpcLimitExceeded`).

---

### 2️⃣ `terraform state mv` (Refactoring Across Addresses and States)

Renaming resources or moving them into child modules without triggering recreation:

```bash
# Rename resource inside the same state file
terraform state mv aws_security_group.old_sg aws_security_group.web_tier_sg

# Move resource into a nested child module
terraform state mv aws_instance.web module.compute.aws_instance.web

# Move resource between two completely separate remote state files
terraform state mv \
  -state-out=../networking/terraform.tfstate \
  aws_vpc.core \
  aws_vpc.core
```

---

## 4. Emergency State Surgery Safety Protocol (SRE Playbook)

Any modification directly targeting state files carries catastrophic blast-radius risk. Follow this exact 5-step checklist before touching production state:

```bash
# -------------------------------------------------------------
# STEP 1: Acquire state lock and pull an immutable local backup
# -------------------------------------------------------------
terraform state pull > state-backup-$(date +%Y%m%d_%H%M%S).json

# -------------------------------------------------------------
# STEP 2: Verify backup integrity
# -------------------------------------------------------------
jq '.serial' state-backup-*.json
jq '.resources | length' state-backup-*.json

# -------------------------------------------------------------
# STEP 3: Perform the surgical state operation
# -------------------------------------------------------------
terraform state rm aws_instance.scratch_vm

# -------------------------------------------------------------
# STEP 4: Validate plan diff against the cloud
# -------------------------------------------------------------
terraform plan -refresh-only

# -------------------------------------------------------------
# STEP 5: Emergency Rollback (If surgery damaged the wrong resource)
# -------------------------------------------------------------
# WARNING: Increment the serial by 1 before force-pushing to prevent lineage rejections
jq '.serial += 1' state-backup-*.json > state-restored.json
terraform state push state-restored.json
```

---

## 5. Summary & Decision Matrix

| Requirement | Recommended Tool / Block | Git Auditable? | Risk Level |
| :--- | :--- | :--- | :--- |
| **Stop managing a resource (Retain in Cloud)** | `removed { from = ... lifecycle { destroy = false } }` | **Yes (100% in PR)** | Low |
| **Destroy a deprecated resource via PR** | `removed { from = ... lifecycle { destroy = true } }` | **Yes (100% in PR)** | Controlled |
| **Rename a resource without recreation** | `moved { from = ... to = ... }` | **Yes (100% in PR)** | Zero Downtime |
| **Emergency manual decoupling (Pre-1.7)** | `terraform state rm <address>` | No (Terminal only) | High |
| **Migrate resource to different state file** | `terraform state mv -state-out=...` | No (Terminal only) | High |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - moved Block Refactoring](./02-moved-Block-Refactoring.md) | [Index](../../../README.md) | [04 - Generate Config from Import →](./04-Generate-Config-from-Import.md) |

