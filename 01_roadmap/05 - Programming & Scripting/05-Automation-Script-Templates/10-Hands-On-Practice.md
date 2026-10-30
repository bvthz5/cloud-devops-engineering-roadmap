# 10 - Hands-On Practice: Testing a Disk Watchdog

## Lab Scenario
Execute the disk watchdog template in dry-run mode and verify that filesystem parsing works accurately.

---

## Lab Steps

### Step 1: Execute Disk Usage Check
```bash
df -h / | awk 'NR==2 {print "Root usage is: " $5 " on device " $1}'
```

### Step 2: Test Log Cleanup Find Logic Safely
```bash
# Print files older than 7 days without deleting:
find /tmp -type f -mtime +7
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
