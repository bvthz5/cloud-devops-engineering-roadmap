# 03 - Outputs, Data Sources & Cross-Module References

## 1. Output Values

```hcl
output "vpc_id" {
  description = "The ID of the VPC"
  value       = aws_vpc.main.id
  sensitive   = false  # Set true to hide in CLI output
}

output "private_subnets" {
  value = aws_subnet.private[*].id   # Splat expression
}
```

## 2. Data Sources

Data sources let you **read** information from existing infrastructure or external APIs without managing the resource.

```hcl
# Read the latest Ubuntu AMI
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]  # Canonical

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-22.04-amd64-server-*"]
  }
}

# Use in resource
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
}
```

## 3. `terraform_remote_state` Data Source

```hcl
# Read outputs from another state file
data "terraform_remote_state" "networking" {
  backend = "s3"
  config = {
    bucket = "my-terraform-state"
    key    = "networking/terraform.tfstate"
    region = "us-east-1"
  }
}

# Use the output
resource "aws_instance" "app" {
  subnet_id = data.terraform_remote_state.networking.outputs.private_subnet_id
}
```

## 4. Common Data Sources by Provider

| Provider | Data Source | Use Case |
|---|---|---|
| AWS | `aws_caller_identity` | Get current account ID |
| AWS | `aws_availability_zones` | List available AZs |
| AWS | `aws_vpc` | Look up existing VPC |
| Azure | `azurerm_client_config` | Current subscription info |
| GCP | `google_project` | Current project details |
| K8s | `kubernetes_service_account` | Existing SA details |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Variables](./02-Variables-Types-Validation-and-Precedence.md) | [README](./README.md) | [04 - Locals & Functions](./04-Locals-Expressions-and-Built-in-Functions.md) |
