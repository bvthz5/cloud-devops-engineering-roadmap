# 03 - Pulumi Architecture & State

## 1. Pulumi Engine

```text
Python/TypeScript/Go Code --> Pulumi Engine --> Cloud API calls
                                  |
                                  v
                          Pulumi State (Pulumi Cloud / S3 / Local)
```

## 2. Example (Python)

```python
import pulumi
import pulumi_aws as aws

vpc = aws.ec2.Vpc("main", cidr_block="10.0.0.0/16")
instance = aws.ec2.Instance("web",
    ami="ami-abc123",
    instance_type="t3.micro",
    subnet_id=vpc.id,
    tags={"Name": "pulumi-web"})

pulumi.export("vpc_id", vpc.id)
```

## 3. Pulumi vs Terraform State

| Aspect | Terraform | Pulumi |
|---|---|---|
| State format | JSON | JSON (encrypted by default in Pulumi Cloud) |
| Backend options | S3, GCS, Azure, Consul | Pulumi Cloud, S3, Azure, local |
| State locking | DynamoDB (for S3) | Built-in (Pulumi Cloud) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Constructs & Stacks](./02-CDKTF-Constructs-and-Stacks.md) | [README](./README.md) | [04 - HCL vs GPL Tradeoffs](./04-HCL-vs-General-Purpose-Language-Tradeoffs.md) |
