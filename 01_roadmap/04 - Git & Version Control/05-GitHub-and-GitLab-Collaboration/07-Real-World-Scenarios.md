# 07 - GitHub Collaboration: Real-World Production Scenarios

## Scenario 1: The CODEOWNERS Outage Deadlock

### Incident Summary
A critical zero-day vulnerability required an emergency 1-line update to a Terraform security group. The DevOps engineer on-call created a PR, but GitHub blocked merging because `.github/CODEOWNERS` required approval from `@company/security-team`. The single security engineer was on an international flight with no internet access.

### Resolution & Governance Fix
1. Organization Admins executed an emergency bypass with audited break-glass procedures.
2. Updated CODEOWNERS policy:
   - Configured secondary on-call escalation groups (`@company/security-oncall`).
   - Defined emergency bypass permissions in GitHub repository settings restricted to Senior Principal SREs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - GitHub Releases and Release Automation](./06-GitHub-Releases-and-Release-Automation.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
