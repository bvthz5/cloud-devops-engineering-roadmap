# 09 - Interview Q&A: AWS Native CI/CD

### Q: How does CodeDeploy perform Blue/Green deployments for ECS tasks behind an ALB?
**Answer:** CodeDeploy provisions a new green target group with updated ECS tasks, routes test traffic to green target group for validation, shifts production listener traffic from blue to green target group, and terminates blue tasks after specified wait period.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
