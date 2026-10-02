# 20 — Hands-On Lab 03: OOM Killer Analysis

## Lab Objective
Simulate a memory leak, inspect kernel ring buffer logs using `dmesg` to find the exact OOM killer event, and understand `oom_score_adj` tuning.

## Step-by-Step Instructions

### Step 1: Inspect Current System Memory & Swap
```bash
free -h
```

### Step 2: Search Past Kernel OOM Events
```bash
sudo dmesg -T | grep -iE 'oom-killer|killed process'
```

### Step 3: Inspect `oom_score` of Running System Daemons
```bash
# Check OOM score of sshd daemon
cat /proc/$(pgrep -o sshd)/oom_score
cat /proc/$(pgrep -o sshd)/oom_score_adj
```

### Step 4: Protect Critical Daemon from OOM-Killer
```bash
# Adjust oom_score_adj to -1000
echo -1000 | sudo tee /proc/$(pgrep -o sshd)/oom_score_adj
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [19 - Lab 02: High CPU Spike](./19-Hands-On-Lab-02-High-CPU-Spike-Isolation.md) | [README](./README.md) | [21 - Lab 04: Systemd Crash Loop](./21-Hands-On-Lab-04-Systemd-Service-Crash-Loop.md) |
