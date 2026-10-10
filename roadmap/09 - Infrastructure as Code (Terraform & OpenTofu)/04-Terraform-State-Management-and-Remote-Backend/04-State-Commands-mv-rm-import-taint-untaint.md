# 04 - State Commands: mv, rm, import, taint & untaint

## 1. The Necessity of "State Surgery"

In a perfect declarative workflow, engineers only edit `.tf` files and execute `terraform plan` and `terraform apply`. However, real-world enterprise engineering demands **State Surgery**—the targeted, direct manipulation of the `terraform.tfstate` database without altering live cloud infrastructure.

State surgery is required during:
1. **Large-Scale Architecture Refactoring:** Moving dozens of standalone resources into reusable child modules without destroying and recreating production databases.
2. **Resource Renaming:** Changing an HCL identifier from `aws_instance.web` to `aws_instance.api_gateway`. (Without state manipulation, Terraform treats this as: *Delete the web instance and build a new api gateway from scratch!*).
3. **Adopting Legacy Infrastructure:** Bringing pre-existing, manually clicked cloud resources under Terraform management.
4. **Decommissioning Management:** Evicting a resource from Terraform tracking while keeping the underlying server running in AWS.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE CODE RENAME TRAP: WHY STATE SURGERY IS CRITICAL                        |
+---------------------------------------------------------------------------------------------------+

 CODE CHANGE: You rename "resource aws_db_instance web" to "resource aws_db_instance api"
 
 DEFAULT TERRAFORM INTERPRETATION (DISASTER!):
 ┌────────────────────────────────────────────────────────┐
 │ - DESTROY: aws_db_instance.web (Deletes Database!)    │
 │ + CREATE:  aws_db_instance.api (New Empty Database!)  │
 └────────────────────────────────────────────────────────┘
 Result: Total production data loss!

 WITH STATE SURGERY (terraform state mv OR moved {} block):
 ┌────────────────────────────────────────────────────────┐
 │ ~ UPDATE POINTER: In-memory address updated:           │
 │   "aws_db_instance.web" ──► "aws_db_instance.api"     │
 │ Real-world cloud resource ID (db-09a2b) is UNTOUCHED!  │
 └────────────────────────────────────────────────────────┘
 Result: Zero downtime, zero infrastructure recreation!
```

---

## 2. Surgical Command 1: `terraform state mv` (Move & Rename)

The `state mv` command modifies the internal address pointers inside the state file without executing any cloud provider API calls.

### Scenario A: Renaming a Resource in the Same Module
```bash
# Updates the address pointer from old name to new name:
terraform state mv aws_instance.web aws_instance.api_server
```

### Scenario B: Refactoring Standalone Resources into a Child Module
When you refactor a messy root configuration by moving an existing security group into a newly created `module "security"`:
```bash
# Moves the state node into the module's hierarchical path:
terraform state mv aws_security_group.app_sg module.security.aws_security_group.app_sg
```

### Scenario C: Moving Resources Across Iterated Lists or Maps
```bash
# Move an uncounted instance to index 0 of a count array:
terraform state mv aws_instance.web aws_instance.web[0]

# Move from a count index to a for_each map key:
terraform state mv 'aws_instance.web[0]' 'aws_instance.web["us-east-1a"]'
```

---

## 3. The Modern Declarative Alternative: `moved` Blocks (Terraform 1.1+)

Running `terraform state mv` imperatively on local terminals is dangerous because every developer and CI/CD runner must coordinate running the exact command at the exact same moment.

Terraform 1.1 introduced **`moved` blocks**, allowing state refactoring to be declared **directly in the HCL source code**:

```hcl
# File: migrations.tf
moved {
  from = aws_instance.web_server
  to   = aws_instance.api_gateway
}

moved {
  from = aws_security_group.legacy_sg
  to   = module.networking.aws_security_group.app_sg
}
```

### How `moved` Blocks Work:
1. When you commit the `moved` block to Git and run `terraform plan`, Terraform detects the historical address in state.
2. It automatically mutates the state pointer during the plan phase, displaying a clean update message:
   ```text
   # aws_instance.web_server has moved to aws_instance.api_gateway
   Plan: 0 to add, 0 to change, 0 to destroy.
   ```
3. All team members and CI/CD pipelines transition smoothly without typing a single manual CLI command.

---

## 4. Surgical Command 2: `terraform state rm` (Evict Without Destroying)

The `state rm` command permanently removes a resource from the state file **without deleting the real-world resource in the cloud**.

```bash
# Stop tracking an old legacy database:
terraform state rm aws_db_instance.legacy_database
```

### Real-World Use Cases:
1. **Offloading Management to Another Team:** A database was originally built by the DevOps team, but ownership is being handed to the Database Administration (DBA) team who manage it through Ansible or a separate state file.
2. **Emergency Break-Glass Recovery:** A cloud resource was corrupted or manually modified such that the provider crashes whenever it attempts to read it. Evicting it with `state rm` unblocks the rest of the pipeline.
3. **Decommissioning Terraform Management:** Allowing infrastructure to survive while retiring the Terraform project.

---

## 5. Surgical Command 3: `terraform import` (Adopting Legacy Infrastructure)

Bringing pre-existing cloud resources into Terraform requires two steps: writing the matching HCL code and binding the real cloud ID into state.

### Modern Declarative Import (Terraform 1.5+ `import {}` Blocks):
In modern Terraform, you never use the legacy imperative CLI command. You declare an `import` block directly in your code:

```hcl
# File: imports.tf
import {
  to = aws_s3_bucket.customer_uploads
  id = "enterprise-customer-uploads-2024"
}
```

### Automated Code Generation (`-generate-config-out`):
Instead of reverse-engineering hundreds of lines of HCL by hand, Terraform can automatically write the matching configuration for you:

```bash
# Generates the exact HCL block needed for the imported resource:
terraform plan -generate-config-out=generated_resources.tf
```
Review the generated `generated_resources.tf` file, adjust formatting, and run `terraform apply` to commit the import into state!

---

## 6. SRE Safety Protocol: The State Backup Mandate

> [!CAUTION]
> **Always take a local snapshot backup before performing any state surgery!** A typo in `state mv` or `state rm` can delete state nodes, making recovery extremely difficult.

```bash
# 1. Download an exact snapshot of remote state:
terraform state pull > state-backup-$(date +%Y%m%d%H%M%S).json

# 2. Perform the surgery:
terraform state mv aws_instance.old aws_instance.new

# 3. Emergency Rollback (if an error occurs):
# Force-push the saved snapshot back to the remote backend:
terraform state push state-backup-20240315103000.json
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - State Locking Concurrency and Force Unlock](./03-State-Locking-Concurrency-and-Force-Unlock.md) | [Index](../../../README.md) | [05 - Sensitive Data in State and Encryption →](./05-Sensitive-Data-in-State-and-Encryption.md) |
