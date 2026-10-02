# 09 - Interview Questions & Scenarios

### Q1: What is Infrastructure as Code and why is it critical for DevOps?
**Answer:**
IaC is the practice of defining infrastructure in version-controlled, machine-readable files. It's critical because it provides reproducibility (same code → same infra), enables peer review of infrastructure changes, creates an audit trail via Git history, supports automated testing and validation, and eliminates configuration drift through idempotent operations.

---

### Q2: Explain the difference between declarative and imperative IaC approaches with examples.
**Answer:**
**Declarative** (Terraform, CloudFormation): You describe the desired end state — "I want 3 EC2 instances." The tool figures out the steps. If 2 exist, it creates 1 more.
**Imperative** (Bash, Pulumi): You describe the steps — "Create an instance, then another, then another." If you run it again, it creates 3 more.
The key advantage of declarative is built-in idempotency and drift detection.

---

### Q3: What is configuration drift and how do you prevent it?
**Answer:**
Configuration drift occurs when actual infrastructure state diverges from IaC-defined state, typically due to manual changes via cloud consoles, CLI commands, or other automation. Prevention strategies:
1. Block direct console/CLI access to production
2. Run `terraform plan` on a schedule to detect drift
3. Use AWS Config / Azure Policy for compliance rules
4. Implement IaC-only change policies enforced via IAM restrictions

---

### Q4: Terraform vs OpenTofu — When would you choose one over the other?
**Answer:**
Choose **OpenTofu** when: you need a fully open-source solution (MPL 2.0), you want client-side state encryption, or your organization's policy prohibits BSL-licensed software.
Choose **Terraform** when: you rely on Terraform Cloud / HCP features, your team's existing tooling and training is built around HashiCorp ecosystem, or you need enterprise support from HashiCorp.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
