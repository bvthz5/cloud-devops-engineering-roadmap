# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Overwritten Tag Rollback Disaster

### Context & Incident
A developer pushed a buggy hotfix tagged as `v1.2.0`, overwriting the previous working `v1.2.0` image in AWS ECR. When Kubernetes pods autoscaled, new nodes pulled the buggy image while existing nodes ran the old image, creating an impossible-to-diagnose split-brain outage.

### Root Cause
ECR repository tag mutability was left set to `MUTABLE`!

### Solution: Enforce Tag Immutability
In AWS ECR:
```bash
aws ecr put-image-tag-mutability \
  --repository-name myapp \
  --image-tag-mutability IMMUTABLE
```
Once set to `IMMUTABLE`, the registry rejects any attempt to overwrite an existing tag with HTTP 400!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Storage Optimization & GC](./06-Registry-Storage-Optimization-and-Garbage-Collection.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
