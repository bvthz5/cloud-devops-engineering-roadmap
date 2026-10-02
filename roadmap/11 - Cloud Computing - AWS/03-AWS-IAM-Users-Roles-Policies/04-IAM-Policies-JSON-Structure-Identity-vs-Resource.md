# 04 - IAM Policy JSON Structure & Types

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3Read",
      "Effect": "Allow",
      "Action": [ "s3:GetObject", "s3:ListBucket" ],
      "Resource": [ "arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*" ]
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - IAM Roles & Trust Policies](./03-IAM-Roles-Trust-Policies-and-AssumeRole.md) | [README](./README.md) | [05 - Policy Evaluation Logic](./05-IAM-Policy-Evaluation-Logic-Explicit-Deny-Rule.md) |
