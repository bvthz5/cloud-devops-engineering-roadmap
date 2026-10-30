# Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Terraform State Lock Stuck During Interrupted CI/CD Run

### 🚨 The Production Scenario
A CI/CD pipeline job building infrastructure times out and gets cancelled mid-run. Subsequent `terraform plan` or `apply` runs fail with `Error: Error acquiring the state lock: ConditionalCheckFailedException (Lock Info: ID: ...)`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When Terraform executes, it acquires an exclusive distributed mutex lock on the state backend (e.g. AWS DynamoDB table or Azure Blob lease) to prevent concurrent writes from corrupting the state file. If the process is abruptly killed (`SIGKILL` or runner timeout), Terraform cannot execute its release hook, leaving the lock active in DynamoDB indefinitely.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Verify whether any other engineer or pipeline is actively modifying infrastructure.
- Step 2: Note the `Lock Info: ID: <UUID>` printed in the error message.
- Step 3: Force release the lock using `terraform force-unlock <LOCK_ID>`.
- Step 4: If CLI unlock fails due to permission boundaries, manually inspect and delete the lock record from the DynamoDB table.
- Step 5: Run `terraform refresh` to synchronize the state file before proceeding.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Attempt plan to reproduce and capture the exact Lock ID string
terraform plan

# Safely release the distributed state lock using the captured Lock ID
terraform force-unlock <LOCK_ID>

# Inspect state lock entry metadata directly in DynamoDB
aws dynamodb get-item --table-name terraform-locks --key '{"LockID": {"S": "my-app/terraform.tfstate-md5"}}'

# Reconcile state file with actual cloud resources after an interrupted run
terraform refresh

# Temporary read-only diagnostic plan bypassing state lock (use with caution)
terraform plan -lock=false

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "State locking prevents concurrent writes from corrupting our infrastructure state. When a pipeline is cancelled, the lock remains orphaned in DynamoDB. First, I verify with the team that no apply is actually running. Once verified, I extract the Lock ID from the error output and run `terraform force-unlock <LOCK_ID>`. I then run `terraform plan` to confirm the state is healthy before allowing any pipeline to resume."

---

## 📌 Scenario 2: Massive Infrastructure Drift Detected during Scheduled Terraform Plan

### 🚨 The Production Scenario
A nightly automated `terraform plan` suddenly reports that 15 core production security groups and route tables will be modified or destroyed, but no code changes were committed to Git.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Infrastructure drift occurs when cloud resources are modified out-of-band directly through the cloud console, CLI, or automated emergency hotfixes. Terraform compares the actual real-world state against the code; if someone manually deleted or changed a tag or rule in AWS, Terraform attempts to revert or recreate it to match the code.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check CloudTrail / Azure Activity Log to identify who changed the resource and why.
- Step 2: If the out-of-band change was an emergency operational fix that should be kept, backport the change into the Terraform HCL code.
- Step 3: Run `terraform plan` to verify zero diff after code alignment.
- Step 4: If the out-of-band change was unauthorized or malicious, apply Terraform (`terraform apply`) to overwrite and remediate the drift.
- Step 5: Enforce strict IAM policies (Service Control Policies) removing console write permissions in production.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check if drift exists (exit code 0 = no changes, 2 = drift detected)
terraform plan -detailed-exitcode

# Trace who made the manual modification in the cloud console
aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceName,AttributeValue=sg-0123456789abcdef0

# Inspect real-world state drift without proposing any infrastructure modifications
terraform plan -refresh-only

# Update local state file to acknowledge and record authorized out-of-band changes
terraform apply -refresh-only

# Revert out-of-band drift back to the codified desired state
terraform apply

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When drift occurs, my first action is investigation via CloudTrail to understand if it was an intentional emergency hotfix or an accidental change. If intentional, I backport the configuration into the HCL code and run `terraform plan` until the diff is zero. If it was unauthorized, I run `terraform apply` to enforce the desired Git state. To prevent drift permanently, production write access is restricted exclusively to the CI/CD pipeline role."

---

## 📌 Scenario 3: Accidental Destruction Plan on a Production Database

### 🚨 The Production Scenario
A developer renames a database resource in Terraform HCL. Running `terraform plan` shows: `Plan: 1 to add, 0 to change, 1 to destroy` on the multi-terabyte production RDS instance.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Terraform identifies resources by their address (e.g. `aws_db_instance.main`). Renaming the HCL block to `aws_db_instance.primary` tells Terraform that the old resource was deleted and an entirely new resource must be created. For stateful resources like databases, this results in catastrophic data destruction.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: NEVER run `terraform apply` when a stateful resource is scheduled for destruction.
- Step 2: Add `lifecycle { prevent_destroy = true }` inside the stateful resource block to hard-block accidental deletion.
- Step 3: Use Terraform's `moved` block (introduced in Terraform 1.1+) to inform Terraform of the HCL refactoring.
- Step 4: Alternatively, use `terraform state mv` to rename the resource pointer inside the state file.
- Step 5: Re-run `terraform plan` and verify the diff changes from `1 to destroy` to `0 to add, 0 to destroy`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Locate the exact existing resource address in the state file
terraform state list | grep db_instance

# Rename resource address inside the state file without touching cloud infrastructure
terraform state mv aws_db_instance.main aws_db_instance.primary

# Modern declarative refactoring using moved block in HCL code
cat <<EOF >> main.tf
moved {
  from = aws_db_instance.main
  to   = aws_db_instance.primary
}
EOF

# Verify that plan now shows 0 resources to destroy
terraform plan

# Ensure AWS native database deletion protection is permanently active
aws rds modify-db-instance --db-instance-identifier prod-db --deletion-protection

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Renaming a resource in HCL tells Terraform to destroy the old resource and provision a new one. For databases, this is catastrophic. I prevent this by always enforcing `prevent_destroy = true` in the resource lifecycle block and enabling cloud deletion protection. To refactor cleanly, I use declarative `moved` blocks (e.g., `moved { from = old to = new }`), which updates the state address cleanly with zero downtime and zero resource recreation."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Kubernetes & Orchestration Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../05-Kubernetes-and-Orchestration-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

