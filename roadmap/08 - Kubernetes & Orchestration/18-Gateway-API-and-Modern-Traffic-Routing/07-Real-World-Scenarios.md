# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Shadow Mirror Overload

### Incident Summary
A platform team configured a `RequestMirror` filter on an HTTPRoute to test a new Go payment engine. Within 3 minutes, the staging database backend crashed, locking database connections.

### Root Cause
The mirrored service had not mocked out database writes. It executed real write transactions twice for every customer order!

### Remediation
1. Implemented read-only transaction flags for shadow environments.
2. Verified that mirroring filters only forward downstream traffic and drop mirrored response bodies.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Gateway Implementations](./06-Gateway-API-Implementations-Envoy-Gateway-Cilium-Traefik.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
