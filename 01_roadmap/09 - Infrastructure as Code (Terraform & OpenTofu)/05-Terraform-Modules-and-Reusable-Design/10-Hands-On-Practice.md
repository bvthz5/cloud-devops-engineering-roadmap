# 10 - Hands-On Practice Labs

## Lab 01: Create a Reusable VPC Module

```bash
# 1. Create module structure
mkdir -p modules/vpc
cat > modules/vpc/main.tf <<'EOF'
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  tags = {
    Name        = "${var.name}-vpc"
    Environment = var.environment
  }
}
EOF

# 2. Create variables and outputs
# modules/vpc/variables.tf & modules/vpc/outputs.tf

# 3. Call from root module
cat > main.tf <<'EOF'
module "vpc" {
  source      = "./modules/vpc"
  vpc_cidr    = "10.0.0.0/16"
  name        = "lab"
  environment = "dev"
}
output "vpc_id" { value = module.vpc.vpc_id }
EOF

# 4. Deploy
terraform init && terraform apply
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
