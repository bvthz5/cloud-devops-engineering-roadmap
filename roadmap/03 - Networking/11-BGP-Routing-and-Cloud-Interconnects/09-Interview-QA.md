# 09 - BGP Routing: Interview Questions & Answers

### Q1: What does it mean when a BGP session is stuck in the "Active" state?
**Answer:** In BGP, "Active" is misleading—it means the router is actively attempting to establish the underlying TCP connection on port 179 to the neighbor, but the connection is failing (connection refused, timed out, or blocked by an intermediate firewall).

### Q2: How can an engineer influence inbound traffic from the internet without contacting their ISP?
**Answer:** By applying **AS-Path Prepending**. By repeating the local ASN multiple times in the BGP route announcement on the secondary link, the secondary route appears longer, causing remote autonomous systems to prefer the shorter primary link.

### Q3: What is the purpose of BGP Route Reflectors in an iBGP network?
**Answer:** The iBGP split-horizon rule requires an internal network to maintain a full-mesh peering ($N(N-1)/2$). A Route Reflector (RR) relaxes this requirement by permitting designated routers to reflect iBGP learned routes to other iBGP clients, drastically simplifying scaling.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
