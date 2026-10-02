# 07 - Real-World Scenarios

## Scenario 01: Drift Detection Saves Production

### Incident
A junior engineer opened port 22 to 0.0.0.0/0 on a production security group via the AWS Console for debugging and forgot to revert it. A scheduled `terraform plan` drift check caught the change 4 hours later.

### Resolution
1. Alert triggered in Slack with plan output showing the drift
2. On-call engineer reviewed and ran `terraform apply` to revert to IaC state
3. Implemented AWS Config rule to detect open SSH rules in real-time

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Policy as Code](./06-Policy-as-Code-Sentinel-OPA-and-Conftest.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
