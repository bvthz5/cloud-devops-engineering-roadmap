# 07 - BGP Routing: Real-World Production Scenarios

## Scenario 1: The Facebook Global BGP Blackout

### Incident Summary
On October 4, 2021, Facebook, Instagram, and WhatsApp disappeared from the global internet for over 6 hours.

### Root Cause
During routine backbone maintenance, a command was issued to evaluate global backbone capacity. The script contained a flaw that severed all connections between Facebook's datacenters and their edge DNS server facilities. Because Facebook's edge DNS servers could no longer communicate with the core datacenters, the DNS servers automatically **withdrew all BGP route announcements** to prevent bad routing. The global internet instantly purged Facebook's IP prefixes from routing tables.

### SRE Lessons Learned
1. Out-of-band management networks (remote console access) must never share BGP or DNS dependencies with the primary production backbone.
2. Automated configuration validation must simulate multi-region routing withdrawal before applying changes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Dynamic Routing Protocols OSPF vs BGP](./06-Dynamic-Routing-Protocols-OSPF-vs-BGP.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
