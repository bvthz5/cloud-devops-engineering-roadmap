# 09 - Interview Q&A: AWS IAM

### Q: What happens if a resource has an Allow in an IAM Policy but a Deny in a Service Control Policy (SCP)?
**Answer:** The request is DENIED. An explicit deny at any evaluation layer (SCP, IAM policy, Permission Boundary) overrides all allows.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
