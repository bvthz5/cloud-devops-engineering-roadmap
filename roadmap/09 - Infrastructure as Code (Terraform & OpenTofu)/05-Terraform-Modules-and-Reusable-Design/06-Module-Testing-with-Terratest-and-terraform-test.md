# 06 - Module Testing: Terratest & terraform test

## 1. Native Testing with `terraform test` (≥ 1.6)

```hcl
# tests/vpc.tftest.hcl
run "create_vpc" {
  command = apply

  variables {
    vpc_cidr    = "10.0.0.0/16"
    environment = "test"
  }

  assert {
    condition     = aws_vpc.main.cidr_block == "10.0.0.0/16"
    error_message = "VPC CIDR block mismatch"
  }

  assert {
    condition     = aws_vpc.main.tags["Environment"] == "test"
    error_message = "Environment tag not set correctly"
  }
}
```

```bash
terraform test
```

## 2. Integration Testing with Terratest (Go)

```go
package test

import (
    "testing"
    "github.com/gruntwork-io/terratest/modules/terraform"
    "github.com/stretchr/testify/assert"
)

func TestVpcModule(t *testing.T) {
    t.Parallel()
    
    opts := &terraform.Options{
        TerraformDir: "../modules/vpc",
        Vars: map[string]interface{}{
            "vpc_cidr":    "10.0.0.0/16",
            "environment": "test",
        },
    }
    
    defer terraform.Destroy(t, opts)
    terraform.InitAndApply(t, opts)
    
    vpcId := terraform.Output(t, opts, "vpc_id")
    assert.Regexp(t, `^vpc-`, vpcId)
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Publishing Modules](./05-Publishing-Modules-to-Terraform-Registry.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
