# 04 - Integration Testing: Terratest & Kitchen-Terraform

## 1. Terratest (Go)

```go
func TestVpcModule(t *testing.T) {
    t.Parallel()
    opts := &terraform.Options{
        TerraformDir: "../modules/vpc",
        Vars: map[string]interface{}{
            "vpc_cidr": "10.99.0.0/16",
        },
    }
    defer terraform.Destroy(t, opts)
    terraform.InitAndApply(t, opts)

    vpcId := terraform.Output(t, opts, "vpc_id")
    assert.Regexp(t, `^vpc-`, vpcId)

    // Verify via AWS SDK
    subnets := aws.GetSubnetsForVpc(t, vpcId, "us-east-1")
    assert.Equal(t, 3, len(subnets))
}
```

## 2. Test Workflow

```text
1. terraform init + apply (create real resources)
2. Run assertions against actual state and cloud APIs
3. terraform destroy (cleanup)
4. Report pass/fail
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Native Testing terraform test](./03-Native-Testing-terraform-test.md) | [Index](../../../README.md) | [05 - Drift Detection Strategies and Automation →](./05-Drift-Detection-Strategies-and-Automation.md) |
