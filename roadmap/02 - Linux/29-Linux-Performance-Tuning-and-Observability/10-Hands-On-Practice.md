# 10 — Hands-On Practice Labs: Performance Profiling

---

## Lab 1: Simulating Workloads with `stress-ng` and Profiling Bottlenecks

### Objective
Generate controlled CPU, memory, and disk stress and use performance tools to identify which resource is saturated.

```bash
# 1. Install stress-ng and sysstat
sudo apt-get install -y stress-ng sysstat

# 2. Simulate 100% CPU saturation on 4 cores for 30 seconds
stress-ng --cpu 4 --timeout 30s &

# Observe with mpstat in real time:
mpstat -P ALL 1 5

# 3. Simulate heavy disk I/O saturation
stress-ng --hdd 2 --timeout 30s &

# Observe await and %util with iostat:
iostat -xz 1 5
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
