# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The 5-Second DNS Timeout Catastrophe

### Incident Summary
A major fintech platform running on Linux kernel 4.x suffered intermittent 5-second connection delays across payment services during peak transaction hours.

### Root Cause
1. Linux kernel Netfilter conntrack bug (`insert_failed` due to race condition when UDP queries for both `A` and `AAAA` records were sent from the same socket concurrently).
2. The second query was dropped by Netfilter, forcing the client resolver to hit its default **5-second retransmit timeout**.

### Remediation
1. Deployed **NodeLocal DNSCache** DaemonSet: Runs a DNS caching agent on every node, converting external UDP traffic to local TCP queries.
2. Configured `single-request-reopen` in `/etc/resolv.conf` to force sequential A and AAAA socket queries.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - EndpointSlices High Scale Service Endpoints](./06-EndpointSlices-High-Scale-Service-Endpoints.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
