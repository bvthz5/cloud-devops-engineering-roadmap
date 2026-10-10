# 01 - AWS Identity & Access Management (IAM) Architecture

> **Focus:** AWS IAM architecture, authentication vs. authorization, root account hardening, security principles (Least Privilege), and identity entities (Users, Groups, Roles, Policies).

---

## 🔐 1. AWS IAM Core Architecture

AWS **Identity and Access Management (IAM)** is a global service that controls who is authenticated (signed in) and authorized (has permissions) to execute actions against AWS resources:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        AWS IAM CORE ARCHITECTURE                       │
│                                                                        │
│   AUTHENTICATION (Who are you?)        AUTHORIZATION (What can you do?)│
│   ├── IAM Users (Credentials, MFA)     ├── JSON Policies               │
│   ├── IAM Groups (Team collections)    ├── Managed vs. Inline Policies │
│   └── IAM Roles (Temporary tokens STS) └── Boundary & SCP Permissions  │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ Evaluates Every API Request
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   AWS SERVICE ENGINE (EC2, S3, RDS)                    │
│             Default: DENY ➔ Explicit ALLOW ➔ Final Action              │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ 2. The 5 Core IAM Security Best Practices

1. **Lock the Root Account Immediately:** The root account (email used to create AWS account) has unrestricted god-mode privileges. Lock it with hardware MFA, delete all access keys, and never use it for daily administrative tasks.
2. **Enforce Multi-Factor Authentication (MFA):** Require FIDO security keys or TOTP authenticator apps for every human user.
3. **Grant Least Privilege:** Start with zero access. Grant only the exact actions (`s3:GetObject`) on specific resources (`arn:aws:s3:::mybucket/*`).
4. **Use IAM Roles for Applications:** Never embed AWS Access Keys (`AKIA...`) inside code, EC2 instances, or Dockerfiles. Attach an IAM Role with temporary rotating credentials via AWS STS!
5. **Rotate Credentials Regularly:** Audit and deactivate unused credentials older than 90 days with IAM Credential Reports.

---

### 💻 Production AWS CLI IAM Commands

```bash
# 1. Create a Developer IAM User
aws iam create-user --user-name devops-engineer

# 2. Create IAM Group and Add User
aws iam create-group --group-name Developers
aws iam add-user-to-group --group-name Developers --user-name devops-engineer

# 3. Generate Credential Report to audit inactive keys
aws iam generate-credential-report
aws iam get-credential-report --output text --query "Content" | base64 -d
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-AWS-Global-Infrastructure-Regions-AZs)](../02-AWS-Global-Infrastructure-Regions-AZs/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - IAM Users Groups and Access Keys →](./02-IAM-Users-Groups-and-Access-Keys.md) |
