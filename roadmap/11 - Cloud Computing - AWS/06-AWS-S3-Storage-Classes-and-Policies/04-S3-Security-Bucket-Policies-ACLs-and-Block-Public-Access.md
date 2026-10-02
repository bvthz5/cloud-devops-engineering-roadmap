# 04 - S3 Security: Bucket Policies & Block Public Access

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EnforceTLSRequestsOnly",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::my-secure-bucket",
        "arn:aws:s3:::my-secure-bucket/*"
      ],
      "Condition": {
        "Bool": { "aws:SecureTransport": "false" }
      }
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - S3 Lifecycle Rules Expiration and Transition Policies](./03-S3-Lifecycle-Rules-Expiration-and-Transition-Policies.md) | [Index](../../../README.md) | [05 - S3 Encryption SSE S3 SSE KMS SSE C and Client Side →](./05-S3-Encryption-SSE-S3-SSE-KMS-SSE-C-and-Client-Side.md) |
