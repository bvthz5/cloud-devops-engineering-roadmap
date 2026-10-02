# 02 - Service Control Policies (SCPs)

SCPs specify maximum allowable permissions for accounts in an Organization or OU.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PreventDisablingGuardDuty",
      "Effect": "Deny",
      "Action": [ "guardduty:DisableOrganizationAdminAccount", "guardduty:DeleteDetector" ],
      "Resource": "*"
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AWS Organizations Multi Account Strategy and OUs](./01-AWS-Organizations-Multi-Account-Strategy-and-OUs.md) | [Index](../../../README.md) | [03 - AWS Control Tower and Landing Zone Architecture →](./03-AWS-Control-Tower-and-Landing-Zone-Architecture.md) |
