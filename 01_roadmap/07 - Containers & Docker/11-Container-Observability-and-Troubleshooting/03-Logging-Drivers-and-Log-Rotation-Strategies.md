# 03 - Logging Drivers and Log Rotation Strategies

## 1. Non-Blocking Logging (`mode=non-blocking`)

By default, Docker's logging driver is **blocking**: if the logging daemon (e.g., Fluentd or Syslog) experiences network congestion, the container's standard I/O buffer fills up and the application process **completely freezes**!

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "25m",
    "max-file": "4",
    "mode": "non-blocking",
    "max-buffer-size": "4m"
  }
}
```
Setting `mode: non-blocking` ensures the container never blocks on logging; if the buffer overflows, older logs are dropped to protect application availability.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Container Exit Codes and Crash Forensics](./02-Container-Exit-Codes-and-Crash-Forensics.md) | [Index](../../../README.md) | [04 - Container Health Monitoring with cAdvisor and Prometheus →](./04-Container-Health-Monitoring-with-cAdvisor-and-Prometheus.md) |
