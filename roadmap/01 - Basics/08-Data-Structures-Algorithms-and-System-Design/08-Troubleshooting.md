# 08 — Algorithmic Performance Troubleshooting Guide

---

## 1. Profiling Time Complexity in Automation Scripts

```bash
# Measure high-level user, sys, and wall-clock execution time
time python3 my_script.py

# Profile Python script execution line by line to locate nested bottlenecks
python3 -m cProfile -s tottime my_script.py | head -n 25
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
