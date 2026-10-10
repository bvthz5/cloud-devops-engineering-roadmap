# 08 - Troubleshooting AWS CLI, Credentials & API Errors

> **Focus:** Diagnosing and resolving signature mismatches, expired STS tokens, IAM AccessDenied errors, clock drift, and VPC endpoint connectivity issues.

---

## 🩺 Top 5 AWS API Errors & Fixes

### 1. `RequestTimeTooSkewed` / `SignatureDoesNotMatch`
- **Cause:** Local operating system clock has drifted by more than 5 minutes from NTP standard time. AWS API signature calculation requires accurate timestamps to prevent replay attacks.
- **Fix:** Resynchronize system clock via NTP:
  ```bash
  # Linux
  sudo chronyd -q 'server pool.ntp.org iburst'
  # Windows PowerShell
  w32tm /resync
  ```

### 2. `ExpiredTokenException: The security token included in the request is expired`
- **Cause:** Temporary credentials generated via AWS STS (AssumeRole or MFA session) reached their maximum session duration (typically 1 hour).
- **Fix:** Re-authenticate via AWS SSO:
  ```bash
  aws sso login --profile dev-engineer
  ```

### 3. `AccessDeniedException: User is not authorized to perform: s3:ListBucket`
- **Cause:** Missing IAM permission, explicit deny in SCP, or S3 bucket policy restriction.
- **Fix:** Decode authorization failure using AWS STS:
  ```bash
  aws sts decode-authorization-message --encoded-message <long-encoded-string>
  ```

### 4. `Could not connect to the endpoint URL`
- **Cause:** Missing internet gateway route, missing DNS resolver, or outbound security group blocking port 443.
- **Fix:** Check outbound connectivity to AWS regional endpoint:
  ```bash
  curl -v https://s3.us-east-1.amazonaws.com
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
