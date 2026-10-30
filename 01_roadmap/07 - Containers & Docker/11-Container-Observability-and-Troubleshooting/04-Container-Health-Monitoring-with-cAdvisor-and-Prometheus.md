# 04 - cAdvisor and Prometheus Container Metrics

## 1. Google cAdvisor Architecture

**cAdvisor (Container Advisor)** runs as a daemon that analyzes resource usage and performance characteristics of running containers directly from the Linux cgroup filesystem.

### Key Prometheus Alerting Metrics:
```promql
# 1. Alert on High CPU Throttling Rate (> 25% of periods throttled)
rate(container_cpu_cfs_throttled_periods_total[5m]) / rate(container_cpu_cfs_periods_total[5m]) > 0.25

# 2. Alert on Container Approaching Memory Hard Limit (> 90%)
container_memory_working_set_bytes / container_spec_memory_limit_bytes > 0.90
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Logging Drivers and Log Rotation Strategies](./03-Logging-Drivers-and-Log-Rotation-Strategies.md) | [Index](../../../README.md) | [05 - Live Debugging with Ephemeral Containers and nsenter →](./05-Live-Debugging-with-Ephemeral-Containers-and-nsenter.md) |
