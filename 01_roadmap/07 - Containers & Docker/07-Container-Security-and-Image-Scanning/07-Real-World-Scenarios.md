# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Exposed Docker TCP Port 2375 Botnet Outage

### Context & Incident
A platform engineer enabled remote Docker API management by adding `-H tcp://0.0.0.0:2375` to the `dockerd` configuration. Within 12 hours, automated scanners on the internet discovered the open port, contacted the Docker REST API, launched privileged crypto-mining containers, and took over the entire company cloud account.

### Root Cause
Port 2375 is unauthenticated plaintext Docker API. Anyone who can reach this port has **full root command execution over the host OS**!

### Solution: Secure TLS on Port 2376 or SSH Tunnel
Never expose port 2375 to the internet! Use Mutual TLS on port 2376 with client certificates, or connect over encrypted SSH:
```bash
export DOCKER_HOST="ssh://deploy@production-server.company.com"
docker ps # Securely executes over SSH!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - CIS Docker Benchmark and Runtime Auditing](./06-CIS-Docker-Benchmark-and-Runtime-Auditing.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
