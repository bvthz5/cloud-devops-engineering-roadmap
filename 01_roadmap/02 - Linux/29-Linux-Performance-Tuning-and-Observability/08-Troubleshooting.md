# 08 — The 60-Second Linux Performance Triage Runbook

Created by performance authority Brendan Gregg, execute these 10 commands in sequence during any live production outage to evaluate system health within 60 seconds.

---

## The 60-Second Checklist

```bash
# 1. Check load average and uptime (Is system saturated?)
uptime

# 2. Check kernel ring buffer for hardware/OOM errors
dmesg -T | tail -n 50

# 3. Check virtual memory, run queue, and swap activity every 1s
vmstat 1 5
# Look at: 'r' (runnable queue), 'b' (blocked on I/O), 'si'/'so' (swap in/out)

# 4. Check multi-core CPU balance and %iowait
mpstat -P ALL 1 1

# 5. Check top CPU-consuming processes
pidstat 1 3

# 6. Check disk I/O latency (await) and saturation (%util)
iostat -xz 1 3

# 7. Check available RAM and buffer/cache
free -m

# 8. Check network interface throughput and packet drops
sar -n DEV 1 1

# 9. Check TCP socket states and connection queues
ss -s

# 10. Check top live processes
top -b -n 1 | head -n 25
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
