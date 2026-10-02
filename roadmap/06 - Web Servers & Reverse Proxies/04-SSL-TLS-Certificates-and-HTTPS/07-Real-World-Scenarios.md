# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Cascading Wildcard Expiration Outage

### Context & Incident
A SaaS platform utilized a wildcard certificate (`*.prod.company.com`) across 80 Kubernetes ingress controllers and load balancers. At 00:00 UTC, the certificate expired. Mobile apps refused connections, API integrations failed, and customer support was overwhelmed.

### Root Cause
1. The certificate was manually renewed once a year via a commercial CA rather than automated via ACME/Let's Encrypt.
2. The notification email was tied to an engineer who had left the company 6 months prior.
3. No automated Blackbox Prometheus alerting existed to warn of upcoming certificate expiration dates.

### Architectural Solution
1. **Automate with cert-manager / ACME DNS-01**: Let's Encrypt automated renewals 30 days prior to expiration.
2. **Prometheus Blackbox Exporter Alerting Rule**:
```yaml
- alert: TlsCertificateExpiringSoon
  expr: probe_ssl_earliest_cert_expiry - time() < 86400 * 14
  for: 1h
  labels:
    severity: warning
  annotations:
    summary: "TLS certificate for {{ $labels.instance }} expires in less than 14 days!"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - SSL Labs A+ Hardening](./06-SSL-Labs-A-Plus-Configuration-and-Cipher-Suites.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
