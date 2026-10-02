# 03 - Native Testing: terraform test

## 1. Test File Structure

```text
project/
+-- main.tf
+-- variables.tf
+-- outputs.tf
+-- tests/
    +-- valid_vpc.tftest.hcl
    +-- invalid_input.tftest.hcl
```

## 2. Plan-Mode Test (No Real Resources)

```hcl
# tests/valid_vpc.tftest.hcl
run "plan_vpc" {
  command = plan    # Only plan, don't apply

  variables {
    vpc_cidr    = "10.0.0.0/16"
    environment = "test"
  }

  assert {
    condition     = aws_vpc.main.cidr_block == "10.0.0.0/16"
    error_message = "VPC CIDR does not match expected value"
  }
}
```

## 3. Apply-Mode Test (Real Resources)

```hcl
run "deploy_and_verify" {
  command = apply   # Actually create resources

  variables {
    vpc_cidr    = "10.99.0.0/16"
    environment = "test"
  }

  assert {
    condition     = output.vpc_id != ""
    error_message = "VPC ID should not be empty after apply"
  }
}
```

```bash
terraform test            # Run all tests
terraform test -verbose   # Detailed output
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Static Analysis tflint terraform validate fmt](./02-Static-Analysis-tflint-terraform-validate-fmt.md) | [Index](../../../README.md) | [04 - Integration Testing Terratest and Kitchen Terraform →](./04-Integration-Testing-Terratest-and-Kitchen-Terraform.md) |
