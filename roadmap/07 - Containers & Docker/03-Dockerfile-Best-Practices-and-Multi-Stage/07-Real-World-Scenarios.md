# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Leaked AWS Credentials in Image History

### Context & Incident
A company pushed a Docker image to Docker Hub. Within 4 hours, crypto-mining instances spawned in their AWS account, generating a $45,000 bill.

### Root Cause
The Dockerfile contained:
```dockerfile
FROM python:3.11
ARG AWS_SECRET_KEY=AKIAIOSFODNN7EXAMPLE
RUN aws s3 cp s3://private-bucket/app.tar.gz . && rm -rf ~/.aws
```
Even though the key was deleted in the same `RUN` command and defined as an `ARG`, anyone who ran `docker history --no-trunc <image>` could read the plaintext AWS credentials stored in the build metadata!

### Solution: BuildKit Secret Mounts
Use `--mount=type=secret`, which mounts secrets into memory without persisting them in any image layer.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - BuildKit Advanced Features](./06-BuildKit-Advanced-Features-and-Cache-Mounts.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
