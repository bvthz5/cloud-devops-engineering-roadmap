# 10 - Hands-On Practice Lab: AWS CLI Setup & Cost Estimation

> **Lab Objective:** Install and configure AWS CLI v2 with named profiles, query regional services, and model an infrastructure deployment using the AWS Pricing Calculator.

---

### 🧪 Task 1: Configure AWS CLI v2 with Named Profiles
```bash
# 1. Configure default profile
aws configure --profile developer
# Enter Access Key, Secret Key, default region (us-east-1), output (json)

# 2. Verify active identity
aws sts get-caller-identity --profile developer

# 3. Query accessible S3 buckets
aws s3 ls --profile developer
```

### 🧪 Task 2: Build a Cloud TCO Cost Model
1. Navigate to the [AWS Pricing Calculator](https://calculator.aws/).
2. Add an **Amazon EC2** service: 2 instances of `m6i.large` (2 vCPU, 8 GB RAM) in `us-east-1`.
3. Compare On-Demand pricing ($69.12/mo) vs. 1-Year Compute Savings Plan ($49.06/mo).
4. Add **Amazon RDS PostgreSQL**: `db.m6i.large` with Multi-AZ deployment.
5. Export estimate as CSV and review the monthly cost delta.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
