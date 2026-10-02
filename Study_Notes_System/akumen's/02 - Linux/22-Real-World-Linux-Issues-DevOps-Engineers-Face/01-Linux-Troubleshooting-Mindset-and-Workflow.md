# 01 — Linux Troubleshooting Mindset and Workflow

## 1. Concept & Philosophy
When a Linux production server experiences an outage or performance degradation, novice engineers often guess at causes and randomly restart services. Senior DevOps/SRE engineers use a **systematic, hypothesis-driven incident response workflow**.

```
+-------------------------------------------------------------+
|                 1. Incident Notification                    |
|             (Alert / User Report / Outage)                  |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                  2. Triage & Stabilization                  |
|          (Isolate blast radius / Stop degradation)          |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|               3. Check Golden Signals (USE/RED)             |
|       (Utilization, Saturation, Errors, Rate, Latency)      |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|               4. Formulate & Test Hypotheses                |
|        (Check logs, metrics, trace process/network)         |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|            5. Root Cause Isolation & Remediation            |
|              (Apply fix / Rollback / Scale)                 |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|             6. Verification & Post-Mortem                   |
|       (Confirm health, write RCA, implement prevention)     |
+-------------------------------------------------------------+
```

## 2. The USE Method (Utilization, Saturation, Errors)
For every hardware resource (CPU, Memory, Disk, Network):
1. **Utilization**: Percentage of time resource was busy (e.g., 95% CPU busy).
2. **Saturation**: Extra work queued waiting for resource (e.g., Load average 12 on a 4-core machine).
3. **Errors**: Count of error events (e.g., Network packet drops, disk I/O read errors).

## 3. Systematic First-Responder Command Order

```bash
# 1. Check basic system uptime and load average
uptime

# 2. Inspect memory and swap utilization
free -h

# 3. Check filesystem storage capacity
df -h

# 4. Identify high resource processes
top -bn1 | head -n 20

# 5. Check recent kernel error messages
dmesg -T --level=err,warn | tail -n 20

# 6. Check system log journal for active service failures
journalctl -p err..emerg -n 30
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - High CPU Usage & Load Average](./02-High-CPU-Usage-and-Load-Average-Diagnosis.md) |
