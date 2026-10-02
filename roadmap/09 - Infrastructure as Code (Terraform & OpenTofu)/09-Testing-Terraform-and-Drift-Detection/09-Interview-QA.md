# 09 - Interview Questions

### Q1: Describe the IaC testing pyramid.
**Answer:**
From bottom to top: (1) Static analysis (tflint, fmt, validate) - instant, free; (2) Unit tests (terraform test in plan mode) - fast, no resources; (3) Integration tests (terraform test apply mode, Terratest) - slow, real resources; (4) E2E tests (full stack deploy) - slowest, highest confidence.

---

### Q2: How do you detect and remediate configuration drift?
**Answer:**
Run `terraform plan -detailed-exitcode` on a cron schedule (every 4-12 hours). Exit code 2 indicates drift. Alert via Slack/PagerDuty and either auto-remediate with `terraform apply` or flag for human review. Supplement with cloud-native tools like AWS Config for real-time detection.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
