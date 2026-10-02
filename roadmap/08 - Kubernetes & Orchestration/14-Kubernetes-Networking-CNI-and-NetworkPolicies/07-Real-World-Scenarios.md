# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The VXLAN MTU Mismatch Black Hole

### Incident Summary
After migrating to a cloud provider with standard 1500-byte MTU, small HTTP requests (health checks) succeeded, but larger HTTPS payloads (> 1450 bytes) or large JSON API responses hung indefinitely until connection timeout.

### Root Cause
VXLAN encapsulation adds a 50-byte header to every network packet.
- Node interface MTU: `1500`
- Pod interface MTU was incorrectly configured at `1500` (instead of `1450`).
- When a pod transmitted a 1500-byte packet, the CNI encapsulated it to 1550 bytes.
- The underlying cloud network dropped the oversized frame because the DF (Don't Fragment) flag was set!

### Remediation
Configured CNI MTU to `1450` in the Calico/Cilium DaemonSet configuration. Packet loss dropped immediately to 0.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Advanced L7 Network Policies and DNS Filtering](./06-Advanced-L7-Network-Policies-and-DNS-Filtering.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
