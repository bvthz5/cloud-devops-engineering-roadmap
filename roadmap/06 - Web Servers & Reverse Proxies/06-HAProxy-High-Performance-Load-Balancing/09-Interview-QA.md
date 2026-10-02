# 09 - Interview Questions & Architectural Scenarios

### Q1: How does HAProxy achieve lower latency and higher throughput than general-purpose web servers like Nginx?
**Answer**: HAProxy is purpose-built exclusively for proxying and load balancing. It utilizes single-process event loops with lockless multi-threading (`nbthread`), deterministic memory allocation (no runtime buffer allocation), and zero-copy TCP splicing (`splice()` syscall) that transfers data between sockets directly inside the Linux kernel without copying bytes to user space.

### Q2: What are stick-tables in HAProxy and what are their primary use cases?
**Answer**: Stick-tables are fast, in-memory key-value stores built into HAProxy. They track client state (IP addresses, session cookies, TLS session IDs) in real time. Primary use cases include session affinity, sliding-window rate limiting (requests per second), bot mitigation, and tracking HTTP error rates.

### Q3: How does the HAProxy Runtime API differ from standard configuration reloads?
**Answer**: Configuration reloads require parsing files and starting new worker processes. The Runtime API communicates via a Unix domain socket, allowing SREs to dynamically change server states (`DRAIN`, `MAINT`, `READY`), modify weights, and inspect metrics in real time with zero process restarts and zero dropped connections.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
