# 05 - Least Privilege IAM for Terraform

## 1. The Problem with Admin Access

Many teams give Terraform `AdministratorAccess` for convenience. This means a compromised CI pipeline or stolen credentials can destroy the entire AWS account.

## 2. Designing Minimal Policies

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "ec2:CreateVpc",
        "ec2:DeleteVpc",
        "ec2:CreateSubnet",
        "ec2:DeleteSubnet",
        "ec2:CreateSecurityGroup",
        "ec2:DeleteSecurityGroup"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::terraform-state-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": ["dynamodb:GetItem", "dynamodb:PutItem", "dynamodb:DeleteItem"],
      "Resource": "arn:aws:dynamodb:us-east-1:*:table/terraform-locks"
    }
  ]
}
```

## 3. Tools for Policy Generation

| Tool | Description |
|---|---|
| **iamlive** | Records actual AWS API calls and generates minimal IAM policy |
| **pike** | Scans Terraform files and generates required IAM permissions |
| **AWS Access Analyzer** | Analyzes CloudTrail logs to suggest least-privilege policies |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Secret Management Vault AWS SSM SOPS](./04-Secret-Management-Vault-AWS-SSM-SOPS.md) | [Index](../../../README.md) | [06 - Supply Chain Security Provider Signing and Lock Files →](./06-Supply-Chain-Security-Provider-Signing-and-Lock-Files.md) |
