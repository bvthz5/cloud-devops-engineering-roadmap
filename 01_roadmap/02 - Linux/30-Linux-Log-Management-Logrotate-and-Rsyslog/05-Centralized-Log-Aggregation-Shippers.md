# 05 — Centralized Log Aggregation Shippers

In containerized, auto-scaled cloud infrastructure, hosts are ephemeral. If an EC2 instance or Kubernetes node terminates, all local logs in `/var/log` are lost forever. Log shippers forward logs continuously to centralized storage.

---

## 1. Modern Log Shippers Comparison

| Shipper | Language | Resource Footprint | Best Use Case |
| :--- | :--- | :--- | :--- |
| **Vector** | Rust | Ultra-low CPU/Memory | High-throughput cloud pipelines, log transformation |
| **Fluent Bit** | C | Extremely low (~20MB RAM) | Standard for Kubernetes node daemonsets |
| **Promtail** | Go | Low | Native companion for Grafana Loki |
| **Filebeat** | Go | Low to moderate | Native shipper for Elastic/Logstash |

---

## 2. Fluent Bit Example: Shipping to Grafana Loki

Configuration `/etc/fluent-bit/fluent-bit.conf`:
```ini
[INPUT]
    Name        tail
    Path        /var/log/syslog, /var/log/auth.log
    Tag         host.linux

[OUTPUT]
    Name        loki
    Match       host.linux
    Host        loki.internal.company.com
    Port        3100
    Labels      job=linux-syslog, host=${HOSTNAME}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - copytruncate vs create Signals](./04-copytruncate-vs-create-Signals.md) | [Index](../../../README.md) | [06 - Auditing Linux with auditd →](./06-Auditing-Linux-with-auditd.md) |
