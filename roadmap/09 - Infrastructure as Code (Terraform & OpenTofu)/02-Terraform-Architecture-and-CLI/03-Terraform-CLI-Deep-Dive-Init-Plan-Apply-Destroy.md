# 03 - Terraform CLI Deep Dive: init, plan, apply, destroy

## 1. Complete CLI Command Reference

### Core Workflow Commands

```bash
# Initialize working directory
terraform init                     # Standard init
terraform init -upgrade            # Upgrade providers within constraints
terraform init -reconfigure        # Reconfigure backend without migrating state
terraform init -migrate-state      # Migrate state to new backend
terraform init -backend=false      # Skip backend initialization

# Format code
terraform fmt                      # Format current directory
terraform fmt -recursive           # Format all subdirectories
terraform fmt -check               # Check formatting (CI — exit code 1 if unformatted)
terraform fmt -diff                # Show diff of formatting changes

# Validate configuration
terraform validate                 # Validate syntax and internal consistency
terraform validate -json           # JSON output for CI parsing

# Plan changes
terraform plan                     # Interactive plan
terraform plan -out=tfplan         # Save plan to file
terraform plan -destroy            # Plan a destroy operation
terraform plan -target=aws_instance.web  # Plan single resource
terraform plan -var="region=us-west-2"   # Override variable
terraform plan -var-file=prod.tfvars     # Use variable file
terraform plan -parallelism=20    # Concurrent operations (default: 10)
terraform plan -refresh=false      # Skip state refresh
terraform plan -json               # Machine-readable JSON plan

# Apply changes
terraform apply                    # Interactive apply
terraform apply tfplan             # Apply saved plan
terraform apply -auto-approve      # Skip confirmation (CI/CD)
terraform apply -target=aws_instance.web  # Apply single resource
terraform apply -replace=aws_instance.web # Force replacement

# Destroy resources
terraform destroy                  # Interactive destroy
terraform destroy -auto-approve    # Skip confirmation
terraform destroy -target=aws_instance.web  # Destroy single resource
```

### State Management Commands

```bash
terraform state list               # List all resources in state
terraform state show aws_instance.web  # Show resource details
terraform state mv old.name new.name   # Rename resource in state
terraform state rm aws_instance.web    # Remove resource from state (orphan)
terraform state pull               # Download remote state to stdout
terraform state push               # Upload local state to remote backend
```

## 2. Environment Variables

| Variable | Purpose | Example |
|---|---|---|
| `TF_LOG` | Set log level | `TF_LOG=DEBUG terraform plan` |
| `TF_LOG_PATH` | Log to file | `TF_LOG_PATH=./terraform.log` |
| `TF_INPUT` | Disable interactive input | `TF_INPUT=false` |
| `TF_VAR_<name>` | Set variable value | `TF_VAR_region=us-west-2` |
| `TF_CLI_ARGS` | Extra CLI args | `TF_CLI_ARGS="-no-color"` |
| `TF_DATA_DIR` | Override `.terraform` dir | `TF_DATA_DIR=/tmp/tf` |
| `TF_PLUGIN_CACHE_DIR` | Provider cache | `TF_PLUGIN_CACHE_DIR=~/.terraform.d/plugin-cache` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Provider Registry Installation and Version Pinning](./02-Provider-Registry-Installation-and-Version-Pinning.md) | [Index](../../../README.md) | [04 - Backend Configuration Local S3 GCS AzureRM →](./04-Backend-Configuration-Local-S3-GCS-AzureRM.md) |
