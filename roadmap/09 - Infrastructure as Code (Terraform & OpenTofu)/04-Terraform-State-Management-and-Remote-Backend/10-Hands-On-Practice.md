# 10 - Hands-On Practice Labs

## Lab 01: Set Up S3 Remote Backend

```bash
# 1. Create S3 bucket and DynamoDB table (bootstrap)
aws s3api create-bucket --bucket my-tf-state-lab --region us-east-1
aws s3api put-bucket-versioning --bucket my-tf-state-lab \
  --versioning-configuration Status=Enabled
aws dynamodb create-table --table-name tf-locks \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST

# 2. Add backend block and migrate
terraform init -migrate-state
```

## Lab 02: Import an Existing Resource

```bash
# Import a pre-existing S3 bucket
terraform import aws_s3_bucket.existing my-existing-bucket-name
terraform plan  # Should show "No changes"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
