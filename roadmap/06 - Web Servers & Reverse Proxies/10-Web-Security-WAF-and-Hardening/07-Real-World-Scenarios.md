# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The WAF False Positive E-Commerce Black Friday Outage

### Context & Incident
A multi-million dollar e-commerce platform enabled the OWASP Core Rule Set (CRS) in blocking mode right before Black Friday. During peak checkouts, 15% of customers saw an HTTP 403 Forbidden screen when clicking "Submit Order".

### Root Cause
Rule **942100** (SQL Injection Detection) flagged customers whose billing addresses contained common contractions like `O'Connor` or street names with single quotes (`D'Silva Road`). The rule score exceeded the anomaly threshold, immediately blocking legitimate revenue transactions.

### Remediation & Runbook
1. **Never switch WAF rules directly into Blocking mode in production**: Run in **Detection-Only mode** (`SecRuleEngine DetectionOnly`) for at least 14 days to baseline traffic.
2. Add rule exclusion tuning for specific field arguments:
   ```apache
   # Exclude billing address argument from SQLi check
   SecRuleUpdateTargetById 942100 "!ARGS:billing_address"
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Zero-Trust Edge & mTLS](./06-Zero-Trust-Edge-mTLS-and-API-Gateway-Hardening.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
