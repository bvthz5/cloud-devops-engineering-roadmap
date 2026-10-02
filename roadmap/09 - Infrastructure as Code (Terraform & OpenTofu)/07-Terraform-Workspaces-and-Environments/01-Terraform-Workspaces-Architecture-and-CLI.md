# 01 - Terraform Workspaces Architecture & CLI

## 1. What Are Workspaces?

Workspaces allow a single Terraform configuration to manage multiple distinct instances of infrastructure, each with its own state file.

```text
Single Configuration (.tf files)
        |
        +-- workspace: default    -> terraform.tfstate
        +-- workspace: dev        -> terraform.tfstate.d/dev/terraform.tfstate
        +-- workspace: staging    -> terraform.tfstate.d/staging/terraform.tfstate
        +-- workspace: prod       -> terraform.tfstate.d/prod/terraform.tfstate
```

## 2. Workspace CLI Commands

```bash
# List all workspaces (* marks current)
terraform workspace list
# * default
#   dev
#   staging

# Create a new workspace
terraform workspace new dev

# Switch to existing workspace
terraform workspace select prod

# Show current workspace
terraform workspace show

# Delete a workspace (must switch away first)
terraform workspace delete dev
```

## 3. Using Workspace Name in Configuration

```hcl
# Reference the current workspace name
resource "aws_instance" "web" {
  ami           = "ami-abc123"
  instance_type = terraform.workspace == "prod" ? "t3.large" : "t3.micro"

  tags = {
    Name        = "web-${terraform.workspace}"
    Environment = terraform.workspace
  }
}

# Workspace-conditional resource creation
resource "aws_cloudwatch_alarm" "high_cpu" {
  count = terraform.workspace == "prod" ? 1 : 0
  # Only create alarms in production
}
```

## 4. Remote Backend with Workspaces

```hcl
# S3 backend: state path includes workspace name
terraform {
  backend "s3" {
    bucket               = "my-tf-state"
    key                  = "services/api/terraform.tfstate"
    region               = "us-east-1"
    workspace_key_prefix = "environments"
    # State paths:
    # environments/dev/services/api/terraform.tfstate
    # environments/prod/services/api/terraform.tfstate
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-Provisioners-and-Local-Exec)](../06-Provisioners-and-Local-Exec/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Workspaces vs Directory Based Environments →](./02-Workspaces-vs-Directory-Based-Environments.md) |
