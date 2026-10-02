# 07 — Real-World DNS Production Scenarios

---

## Scenario 1: The 86,400s (24-Hour) TTL Disaster Recovery Disaster

### Incident Summary
A primary datacenter experiences power failure. The DevOps team updates DNS records for `api.company.com` to point to the backup cloud disaster recovery region.
Hours later, customer traffic still hits the dead datacenter!

### Root Cause Analysis
The original `A` record had a **TTL of 86,400 seconds (24 hours)**.
Worldwide recursive resolvers (ISPs, Google, corporate firewalls) cached the old IP address and will not query authoritative nameservers until the 24-hour timer expires!

### Prevention Best Practice
- Normal steady-state TTL: `300` to `3600` seconds (5 minutes to 1 hour).
- Before scheduled maintenance or migrations: Lower TTL to `60` seconds **24 hours in advance**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - DNSSEC & Security](./06-DNSSEC-and-DNS-Security.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
