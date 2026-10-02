# 04 - Generate Config from Import

## 1. Workflow

```bash
# Step 1: Add import blocks
cat >> imports.tf <<EOF
import {
  to = aws_vpc.main
  id = "vpc-0abc123"
}
EOF

# Step 2: Generate HCL configuration
terraform plan -generate-config-out=generated.tf

# Step 3: Review and clean up generated.tf
# Remove computed-only attributes
# Add meaningful variable references
# Organize into proper file structure

# Step 4: Apply to update state
terraform apply
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - removed Block](./03-removed-Block-and-State-Surgery.md) | [README](./README.md) | [05 - Large-Scale Migration](./05-Large-Scale-Migration-Strategies.md) |
