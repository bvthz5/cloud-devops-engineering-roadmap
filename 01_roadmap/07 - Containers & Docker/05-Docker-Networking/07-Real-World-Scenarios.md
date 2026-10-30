# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Docker UFW Firewall Bypass Security Breach

### Context & Incident
A company configured Ubuntu's UFW firewall to block port 5432 (PostgreSQL) from the public internet. They launched a container using `docker run -d -p 5432:5432 postgres`. Three weeks later, security researchers discovered their database was openly accessible and indexed on Shodan.

### Root Cause
Docker modifies Linux kernel `iptables` directly before UFW rules are evaluated. Publishing a port (`-p 5432:5432`) routes through Docker's NAT chains, completely bypassing UFW!

### Solution: Bind to Localhost Explicitly
```bash
# SECURE: Binds exclusively to host loopback interface!
docker run -d -p 127.0.0.1:5432:5432 postgres
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Overlay Networks and Multi Host VXLAN](./06-Overlay-Networks-and-Multi-Host-VXLAN.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
