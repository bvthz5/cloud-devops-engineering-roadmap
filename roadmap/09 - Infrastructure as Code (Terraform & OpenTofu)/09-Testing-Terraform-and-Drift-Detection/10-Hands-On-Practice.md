# 10 - Hands-On Practice Labs

## Lab 01: Write a terraform test

Create `tests/basic.tftest.hcl` for your VPC module with plan-mode assertions on CIDR block and tags. Run with `terraform test`.

## Lab 02: Set Up Drift Detection

Create a GitHub Actions workflow with `cron: '0 */6 * * *'` that runs `terraform plan -detailed-exitcode` and sends a Slack notification on exit code 2.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
