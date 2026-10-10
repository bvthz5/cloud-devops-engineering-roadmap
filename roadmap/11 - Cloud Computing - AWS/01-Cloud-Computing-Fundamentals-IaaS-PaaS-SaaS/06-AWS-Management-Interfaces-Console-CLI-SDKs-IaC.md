# 06 - AWS Management Interfaces: Console, CLI v2, SDKs & IaC

> **Focus:** Interacting with AWS APIs, AWS Management Console, AWS CLI v2 credential precedence, AWS Boto3 SDK, and Infrastructure as Code (IaC) tooling.

---

## 🖥️ 1. The 4 AWS Interaction Interfaces

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        HOW YOU TALK TO AWS APIS                        │
│                                                                        │
│  1. Web GUI ────────► AWS Management Console (portal in browser)       │
│  2. Terminal ───────► AWS CLI v2 (Command-line scripts, automation)   │
│  3. Code SDKs ──────► Boto3 (Python), Node.js, Go, Java SDKs          │
│  4. IaC Declarative ► Terraform, AWS CloudFormation, AWS CDK          │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ Every interface calls the exact same
                                    ▼ HTTPS REST API endpoints
┌────────────────────────────────────────────────────────────────────────┐
│                      AWS REGIONAL API ENDPOINTS                        │
└────────────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 2. AWS CLI v2 Credential Precedence Order

When you run an AWS CLI command, AWS searches for credentials in this strict order:

1. **Command Line Flags:** `--access-key`, `--secret-key` (Rare / Anti-pattern).
2. **Environment Variables:** `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`.
3. **AWS IAM Identity Center / SSO Credentials:** Temporary cached token in `~/.aws/sso/cache`.
4. **Shared Credentials File:** `~/.aws/credentials` (Profiles: `[default]`, `[prod]`).
5. **Shared Configuration File:** `~/.aws/config`.
6. **Container Credentials:** ECS task IAM role (`AWS_CONTAINER_CREDENTIALS_RELATIVE_URI`).
7. **EC2 Instance Metadata (IMDSv2):** Attached IAM Instance Profile role.

---

### 💻 Production Python Boto3 SDK Script

```python
import boto3
from botocore.exceptions import ClientError

# Boto3 automatically searches the credential precedence chain!
s3_client = boto3.client('s3', region_name='us-east-1')

try:
    response = s3_client.list_buckets()
    print("Accessible Buckets:")
    for bucket in response['Buckets']:
        print(f" • {bucket['Name']}")
except ClientError as e:
    print(f"Error accessing AWS APIs: {e}")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Capex vs Opex and Cloud Economics](./05-Capex-vs-Opex-and-Cloud-Economics.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
