# 09 - Interview Questions & Architectural Scenarios

### Q1: Why is automatic DNS resolution available on user-defined bridge networks but not on the default `docker0` bridge?
**Answer**: For backward compatibility with legacy Docker implementations. The default `docker0` bridge uses historical `/etc/hosts` injection and requires manual `--link` parameters. User-defined bridges instantiate the internal embedded DNS server (`127.0.0.11`), providing dynamic, automatic container name and alias resolution.

### Q2: How does Docker interact with Linux `iptables` when publishing a port?
**Answer**: Docker inserts rules into the `PREROUTING` and `DOCKER` chains of the `nat` and `filter` tables. Incoming traffic to the host port is translated via Destination NAT (DNAT) to the private container IP and port, bypassing standard UFW host firewall rules unless explicitly intercepted via the `DOCKER-USER` chain.

### Q3: When should `--network host` be used instead of bridge networking?
**Answer**: `--network host` is used when: 1) Maximum network throughput and lowest possible latency are required (e.g. high-frequency trading, real-time video streaming), eliminating NAT routing overhead, 2) Handling broadcast or multicast traffic, 3) Profiling network performance.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
