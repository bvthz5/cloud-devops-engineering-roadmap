# 03 - IAM Roles, Trust Policies, Permissions Boundaries & STS AssumeRole

> **Focus:** IAM Roles vs. Users, Trust Policy (`AssumeRole`) vs. Permissions Policy, Temporary Security Credentials (AWS STS), Cross-Account access, and EC2 Instance Profiles.

---

## 🎭 1. What is an IAM Role?

An **IAM Role** is an identity you create in your account that has specific permissions, but is **not** associated with a specific person. It does not have long-term passwords or access keys. Instead, any trusted entity (EC2, Lambda, external AWS Account) can **assume** the role and receive temporary, auto-rotating credentials via **AWS Security Token Service (STS)**.

```text
┌───────────────────────────────┐
│     EC2 Virtual Machine       │
│  (No hardcoded AWS keys!)     │
└──────────────┬────────────────┘
               │ 1. Requests Token from Instance Metadata Service (IMDSv2)
               ▼
┌───────────────────────────────┐
│       AWS STS Service         │
│  (Evaluates Trust Policy)     │
└──────────────┬────────────────┘
               │ 2. Issues Temporary Access Key, Secret Key & Session Token (1 hr)
               ▼
┌───────────────────────────────┐
│       Amazon S3 Bucket        │
│  (Evaluates Permission Policy)│
└───────────────────────────────┘
```

---

## 📜 2. The Two Policies of an IAM Role

Every IAM Role requires **two separate JSON policies**:

### 1. Trust Policy (Who can wear the hat?)
Defines which principals (EC2, Lambda, another AWS Account) are permitted to call `sts:AssumeRole`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### 2. Permissions Policy (What can they do while wearing the hat?)
Defines the AWS resource actions the principal can perform once the role is assumed:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::production-app-assets/*"
    }
  ]
}
```

---

### 💻 Creating and Attaching an IAM Role to an EC2 Instance Profile

```bash
# 1. Create the IAM Role with the EC2 Trust Policy
aws iam create-role \
  --role-name EC2-S3-Reader-Role \
  --assume-role-policy-document file://ec2-trust-policy.json

# 2. Attach the Managed S3 ReadOnly Policy to the Role
aws iam attach-role-policy \
  --role-name EC2-S3-Reader-Role \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# 3. Create Instance Profile & Attach Role
aws iam create-instance-profile --instance-profile-name EC2-S3-Reader-Profile
aws iam add-role-to-instance-profile \
  --instance-profile-name EC2-S3-Reader-Profile \
  --role-name EC2-S3-Reader-Role

# 4. Associate Instance Profile with Running EC2 VM
aws ec2 associate-iam-instance-profile \
  --instance-id i-0a1b2c3d4e5f67890 \
  --iam-instance-profile Name=EC2-S3-Reader-Profile
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - IAM Users Groups and Access Keys](./02-IAM-Users-Groups-and-Access-Keys.md) | [Index](../../../README.md) | [04 - IAM Policies JSON Structure Identity vs Resource →](./04-IAM-Policies-JSON-Structure-Identity-vs-Resource.md) |
