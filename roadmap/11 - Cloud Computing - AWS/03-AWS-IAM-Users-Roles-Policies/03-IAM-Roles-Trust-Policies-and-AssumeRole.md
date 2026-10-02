# 03 - IAM Roles, Trust Policies & AssumeRole

IAM Roles issue **short-lived temporary credentials** (`sts:AssumeRole`).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "ec2.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - IAM Users Groups and Access Keys](./02-IAM-Users-Groups-and-Access-Keys.md) | [Index](../../../README.md) | [04 - IAM Policies JSON Structure Identity vs Resource →](./04-IAM-Policies-JSON-Structure-Identity-vs-Resource.md) |
