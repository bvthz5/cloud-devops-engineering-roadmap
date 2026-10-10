# 03 - Terraform CLI Deep Dive: init, plan, apply, destroy

## 1. The Core Terraform Command Lifecycle

The Terraform command-line interface (CLI) is structured around a deterministic state machine. Every interaction with real-world infrastructure flows through four foundational phases:

```text
+---------------------------------------------------------------------------------------------------+
|                        THE 4-STAGE TERRAFORM EXECUTION PIPELINE                                   |
+---------------------------------------------------------------------------------------------------+

 ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
 │ terraform init  │──────►│ terraform plan  │──────►│ terraform apply │──────►│terraform destroy│
 └─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘
  Bootstraps backend        Refreshes state,          Acquires lock,            Inverts graph,
  & pulls provider          calculates delta,         walks DAG, calls          destroys resources,
  plugins into local        outputs execution         cloud APIs, updates       cleans state.
  cache directory.          plan (-out=tfplan).       state atomically.
```

---

## 2. Deep Dive: Phase 1 — `terraform init`

The `init` command is the directory bootstrap step. It must be run before any planning or state operations.

### What Happens Under the Hood:
1. **Backend Initialization:** Reads the `backend` configuration in `terraform {}`. If using a remote backend (Amazon S3, Azure Blob, GCS), it verifies network connectivity, bucket existence, and state locking permissions. If backend configuration changed since the last run, it prompts for migration (`-migrate-state`) or reinitialization (`-reconfigure`).
2. **Provider Plugin Resolution:** Parses `required_providers` across all root files and child modules. Queries the configured registry, downloads platform-specific compiled binaries, and places them into `.terraform/providers/`.
3. **Dependency Lock Verification:** Compares the downloaded provider checksums against `.terraform.lock.hcl`. If a checksum does not match, execution halts with a security tampering error.
4. **Module Installation:** Recursively scans `module` blocks, downloads external modules (from Git or registries), and unpacks them into `.terraform/modules/`.

```bash
# Production init flags:
terraform init -upgrade             # Upgrades providers within version constraints
terraform init -reconfigure         # Ignores previous backend settings; reconfigures cleanly
terraform init -migrate-state       # Migrates local state to remote S3 backend safely
terraform init -backend=false       # Skips backend (useful for linting/validation in CI)
```

---

## 3. Deep Dive: Phase 2 — `terraform plan`

The `plan` command creates an execution plan, reading current infrastructure state, comparing it against the desired HCL configuration, and computing the delta.

### Step-by-Step Internal Execution:
1. **Acquires State Read Lock:** Ensures no other process is actively mutating the state file.
2. **State Refresh:** Terraform Core sends a gRPC `ReadResource` RPC to the provider for every single resource currently recorded in the state file. The provider queries the cloud APIs (e.g., calling `ec2:DescribeInstances` on AWS) to fetch the real, live state of each object.
3. **Graph Construction:** Builds the Directed Acyclic Graph (DAG) connecting resources, data sources, and modules.
4. **Delta Calculation:** Computes the mathematical difference:
   $$\Delta = \text{Desired State (HCL)} - \text{Refreshed State (Cloud)}$$
5. **Output Formatting:** Displays planned actions using standard diff symbols:

```text
+---------------------------------------------------------------------------------------------------+
|                        TERRAFORM PLAN DIFF SYMBOL TAXONOMY                                        |
+---------------------------------------------------------------------------------------------------+

 + create:         Resource will be created from scratch.
 ~ update in-place: Resource exists; attributes will be modified without destroying the resource.
 - destroy:        Resource will be permanently deleted.
-/+ replace:       Resource cannot be updated in-place (immutable attribute changed);
                   Terraform must DESTROY the old instance and CREATE a new one!
 <= read:          Data source will be queried to fetch real-world attributes.
```

### The Critical Production Pattern: `-out=tfplan`
In production and automated CI/CD pipelines, **never run `terraform apply` without a pre-saved plan file**.

```bash
# Step 1: Save execution plan to an encrypted disk artifact
terraform plan -out=tfplan

# Step 2: Apply the exact saved plan
terraform apply tfplan
```

### Why `-out=tfplan` Is Mandatory:
Running `terraform apply` interactively without an artifact recalculates the plan at the moment `apply` is invoked. In the time between when an engineer reviews the terminal output and types `yes`, someone else might have created a conflicting cloud resource or modified a dependency (a **Time-of-Check to Time-of-Use / TOCTOU race condition**). A saved plan file freezes the exact plan and guarantees that Terraform executes only the vetted changes.

### Automated CI/CD Detection: `-detailed-exitcode`
In automation scripts, use `-detailed-exitcode` to detect whether infrastructure drift or changes exist:
- **Exit Code 0:** Succeeded, diff is empty (no changes).
- **Exit Code 1:** Command failed due to syntax or network error.
- **Exit Code 2:** Succeeded, and changes are present (infrastructure drift detected!).

---

## 4. Deep Dive: Phase 3 — `terraform apply`

The `apply` command executes the actions proposed in a Terraform plan.

```text
+---------------------------------------------------------------------------------------------------+
|                        terraform apply EXECUTION & STATE MUTATION                                 |
+---------------------------------------------------------------------------------------------------+

 1. Acquires State Exclusive Write Lock (DynamoDB / Storage Lock)
 2. Evaluates Directed Acyclic Graph (DAG) in Parallel (Default: --parallelism=10)
       │
       ├── Branch A: Independent Subnet ──► Calls ec2:CreateSubnet API
       └── Branch B: Independent Security Group ──► Calls ec2:CreateSecurityGroup API
       │
 3. Dispatches gRPC "ApplyResourceChange" RPCs to Provider Plugins
 4. Receives Cloud IDs (e.g., subnet-01a2b, sg-09c8e)
 5. Writes New State to Remote Storage Atomically!
 6. Releases Exclusive State Write Lock
```

### The Modern Replacement for `taint`: `-replace`
In older versions of Terraform, marking a corrupted resource for recreation required running `terraform taint aws_instance.web`. This mutated the state file immediately, which was dangerous in shared environments.

Modern Terraform completely deprecates `taint` in favor of the non-destructive **`-replace`** flag on `plan` and `apply`:

```bash
# Safely forces recreation of the web instance in the next execution plan:
terraform plan -replace="aws_instance.web" -out=tfplan
terraform apply tfplan
```

---

## 5. Deep Dive: Phase 4 — `terraform destroy`

The `destroy` command terminates all managed infrastructure recorded in the state file.

### Safe Teardown Mechanics:
1. `destroy` does not wipe resources randomly; it **inverts the Directed Acyclic Graph**.
2. If Resource B depends on Resource A, Terraform terminates Resource B first, ensuring clean network and storage detachment before destroying Resource A.
3. Once all cloud API deletions succeed, Terraform empties the state file and records that zero resources are managed.

```bash
# Preview what would be destroyed without touching cloud infrastructure:
terraform plan -destroy

# Execute destruction in automated tear-down pipelines:
terraform destroy -auto-approve
```

---

## 6. The Danger of `-target`: Why It Is an Enterprise Anti-Pattern

Engineers frequently encounter `-target` (e.g., `terraform apply -target=aws_instance.web`). 

While useful during emergency break-glass rescue operations, **`-target` is an anti-pattern in normal production operations**:
1. **Bypasses Dependency Consistency:** Targeting a single resource ignores upstream and downstream resources that may require synchronization.
2. **Masks Configuration Drift:** Running `-target` does not refresh or reconcile the rest of your infrastructure, allowing undetected drift to accumulate in state.
3. **Risk of Forgotten Dependencies:** If Resource B references an output from Resource A, targeting only Resource A leaves Resource B in an unapplied, partial state.

> [!WARNING]
> Use `-target` strictly for isolated debugging or emergency recovery when a broken resource blocks the entire DAG. Immediately follow up with a full, untargeted `terraform plan` and `terraform apply`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Provider Registry Installation and Version Pinning](./02-Provider-Registry-Installation-and-Version-Pinning.md) | [Index](../../../README.md) | [04 - Backend Configuration Local S3 GCS AzureRM →](./04-Backend-Configuration-Local-S3-GCS-AzureRM.md) |
