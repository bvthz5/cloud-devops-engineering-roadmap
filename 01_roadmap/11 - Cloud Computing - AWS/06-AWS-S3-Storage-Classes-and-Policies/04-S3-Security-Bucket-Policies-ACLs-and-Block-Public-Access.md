# 04 - S3 Security: S3 Block Public Access, Bucket Policies & Pre-Signed URLs

> **Focus:** Zero-trust S3 security, S3 Block Public Access settings, Resource-based Bucket Policies, and Pre-Signed URLs for secure time-limited client uploads.

---

## 🛡️ 1. S3 Block Public Access (The 4 Critical Gates)

S3 Block Public Access provides centralized controls to prevent accidental exposure of buckets to the public internet:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   S3 BLOCK PUBLIC ACCESS SETTINGS                      │
│                                                                        │
│  [X] BlockPublicAcls       ➔ Blocks public ACLs on new objects         │
│  [X] IgnorePublicAcls      ➔ Ignores all existing public ACLs          │
│  [X] BlockPublicPolicy     ➔ Rejects new public bucket policies        │
│  [X] RestrictPublicBuckets ➔ Restricts public bucket access to AWS     │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 📜 2. Production S3 Bucket Policy (Enforce HTTPS TLS 1.2)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EnforceHTTPSOnly",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::production-company-data",
        "arn:aws:s3:::production-company-data/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
```

---

## ⏳ 3. Pre-Signed URLs for Secure Downloads and Uploads

A **Pre-Signed URL** grants temporary, scoped access to an S3 object using the IAM credentials of the URL creator:

```bash
# Generate a Pre-Signed URL valid for 30 minutes (1800 seconds)
aws s3 presign s3://production-company-data/invoices/inv_1092.pdf --expires-in 1800
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - S3 Lifecycle Rules Expiration and Transition Policies](./03-S3-Lifecycle-Rules-Expiration-and-Transition-Policies.md) | [Index](../../../README.md) | [05 - S3 Encryption SSE S3 SSE KMS SSE C and Client Side →](./05-S3-Encryption-SSE-S3-SSE-KMS-SSE-C-and-Client-Side.md) |
