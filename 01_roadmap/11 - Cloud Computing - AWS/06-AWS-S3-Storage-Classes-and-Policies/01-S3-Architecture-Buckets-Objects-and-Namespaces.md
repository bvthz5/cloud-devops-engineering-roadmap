# 01 - Amazon S3 Architecture: Buckets, Objects, Namespaces & Consistency

> **Focus:** Object storage mechanics, Global unique bucket namespace, S3 URI structures, Strong Read-After-Write consistency, and 5 TB single-object limits.

---

## 🪣 1. Amazon S3 Architecture

**Amazon Simple Storage Service (S3)** is an industry-leading object storage service storing trillions of objects and exabytes of data worldwide:

```text
Global S3 Domain: https://<bucket-name>.s3.<region>.amazonaws.com
   │
   └── Bucket: "production-customer-assets-2025" (Globally unique name across ALL AWS accounts)
        │
        └── Object: Key = "photos/profile_42.jpg"
             ├── Data Stream: The binary file payload (0 bytes to 5 TB max)
             ├── Metadata: Key-value pairs (Content-Type: image/jpeg)
             ├── Version ID: Unique string identifying object version
             └── Access Control: Bucket policies & IAM authorization
```

---

## ⚡ 2. Strong Read-After-Write Consistency

Since December 2020, Amazon S3 provides **Strong Read-After-Write Consistency** automatically for all applications at no additional cost:
- After a successful `PUT` of a new object or an `overwrite` of an existing object, any subsequent `GET` or `LIST` request immediately returns the latest version.
- After a successful `DELETE` of an object, subsequent read requests immediately confirm the deletion (HTTP 404).

---

### 💻 Production AWS CLI S3 Commands

```bash
# 1. Create a Bucket in us-east-1
aws s3api create-bucket \
  --bucket enterprise-datalake-prod-2025 \
  --region us-east-1

# 2. Upload an Object with Metadata
aws s3 cp ./data.csv s3://enterprise-datalake-prod-2025/raw/data.csv \
  --metadata "department=finance,classification=confidential"

# 3. Enable Versioning (Protects against accidental deletion)
aws s3api put-bucket-versioning \
  --bucket enterprise-datalake-prod-2025 \
  --versioning-configuration Status=Enabled
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-AWS-EC2-Compute-and-Auto-Scaling)](../05-AWS-EC2-Compute-and-Auto-Scaling/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - S3 Storage Classes Standard IA Glacier Deep Archive →](./02-S3-Storage-Classes-Standard-IA-Glacier-Deep-Archive.md) |
