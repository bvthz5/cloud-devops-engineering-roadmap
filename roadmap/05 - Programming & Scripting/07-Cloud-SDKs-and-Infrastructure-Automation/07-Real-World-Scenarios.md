# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The API Rate-Limit Outage (`RequestLimitExceeded`)

### Context & Incident
A FinTech company deployed an auto-scaling batch processing pipeline in AWS ECS. At 09:00 UTC, 400 container instances spawned simultaneously and began querying AWS Parameter Store (`ssm:GetParameter`) to fetch database credentials and secrets during container initialization.

### Root Cause
AWS Systems Manager Parameter Store defaults to a rate limit of 40 requests per second per account. The sudden surge of 400 simultaneous requests instantly triggered:
`botocore.exceptions.ClientError: An error occurred (ThrottlingException) when calling the GetParameter operation: Rate exceeded`
Because the application had default retry configurations with fixed intervals, the containers retried in lockstep (thundering herd), prolonging the outage for 35 minutes.

### Architectural Solution
1. **Enable SSM Higher Throughput** (supports up to 10,000 req/sec via account setting).
2. **Configure Adaptive Backoff with Jitter in Boto3**:
```python
from botocore.config import Config
import boto3

adaptive_config = Config(
    retries={
        'max_attempts': 8,
        'mode': 'adaptive'  # Automatically applies truncated exponential backoff with full jitter
    }
)
ssm = boto3.client('ssm', config=adaptive_config)
```
3. Cache parameters locally inside the container filesystem with a 15-minute TTL.

---

## Scenario 2: Unhandled STS Token Expiration in Long-Running Data Migrations

### Context & Incident
An engineer ran a 14-hour cross-account S3 backup synchronization script. At hour 1, the script crashed with `ExpiredToken: The security token included in the request is expired`.

### Root Cause
The script invoked `sts.assume_role()` at startup and passed static temporary credentials into the S3 client:
```python
# FLAWED: Credentials expire after 1 hour!
creds = sts.assume_role(...)['Credentials']
s3 = boto3.client('s3', aws_access_key_id=creds['AccessKeyId'], ...)
```
When hour 1 elapsed, the temporary credentials expired, and Boto3 had no mechanism to refresh them.

### Solution: Credential Providers with Auto-Refresh
Configure AWS Profiles or dynamic credential refreshers using `botocore.credentials.DeferredRefreshableCredentials`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Security Scanning & Compliance Automation](./06-Security-Scanning-and-Compliance-Automation-with-SDKs.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
