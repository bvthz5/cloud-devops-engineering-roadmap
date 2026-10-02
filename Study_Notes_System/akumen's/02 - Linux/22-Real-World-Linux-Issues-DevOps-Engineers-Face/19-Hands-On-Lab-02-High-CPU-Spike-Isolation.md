# 19 — Hands-On Lab 02: High CPU Spike Isolation

## Lab Objective
Simulate a runaway CPU-bound process using `sha256sum`, isolate the process PID using `top` and `pidstat`, and dynamically throttle its CPU consumption using `renice` and `cpulimit`.

## Step-by-Step Instructions

### Step 1: Spawn Runaway CPU Background Task
```bash
sha256sum /dev/zero &
RUNAWAY_PID=$!
echo "Runaway PID: $RUNAWAY_PID"
```

### Step 2: Diagnose CPU Consumption
```bash
# Check load average and top CPU process
top -p "$RUNAWAY_PID" -bn1

# Monitor process CPU usage per second
pidstat -u 1 3 -p "$RUNAWAY_PID"
```

### Step 3: Lower CPU Priority using `renice`
```bash
renice -n 19 -p "$RUNAWAY_PID"
```

### Step 4: Terminate Rogue Process
```bash
kill -9 "$RUNAWAY_PID"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [18 - Lab 01: Disk Leak](./18-Hands-On-Lab-01-Unlinked-Open-File-Disk-Leak.md) | [README](./README.md) | [20 - Lab 03: OOM Killer Analysis](./20-Hands-On-Lab-03-OOM-Killer-Analysis.md) |
