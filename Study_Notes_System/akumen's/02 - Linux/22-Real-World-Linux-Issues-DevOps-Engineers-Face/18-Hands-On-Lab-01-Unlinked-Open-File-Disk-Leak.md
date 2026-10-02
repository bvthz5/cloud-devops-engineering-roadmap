# 18 — Hands-On Lab 01: Unlinked Open File Disk Leak

## Lab Objective
Simulate a production incident where a deleted file continues to consume disk space due to an active open file descriptor, diagnose the issue using `lsof`, and reclaim the space without restarting the server.

## Step-by-Step Instructions

### Step 1: Create a Large Test File
```bash
# Create a 500 MB dummy log file in /tmp/
dd if=/dev/zero of=/tmp/leak_test.log bs=1M count=500
```

### Step 2: Simulate Active Process Holding Open File Descriptor
```bash
# Open file descriptor 3 in background tail process
exec 3< /tmp/leak_test.log
tail -f /tmp/leak_test.log &
TAIL_PID=$!
echo "Tail process PID: $TAIL_PID"
```

### Step 3: Delete File Entry
```bash
# Remove directory link
rm -f /tmp/leak_test.log
```

### Step 4: Verify Disk Leak Discrepancy
```bash
# du will NOT show the file (it was deleted from directory index)
du -sh /tmp/leak_test.log 2>/dev/null || echo "File not visible in directory!"

# df shows space is STILL consumed!
df -h /tmp
```

### Step 5: Diagnose with `lsof`
```bash
lsof +L1 /tmp
# Or search deleted files:
lsof | grep "leak_test.log"
```

### Step 6: Remediate by Truncating File Handle or Terminating Process
```bash
# Kill tail process holding file handle
kill -9 "$TAIL_PID"
exec 3<&-

# Verify space restored
df -h /tmp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - Universal Toolkit](./17-Universal-Troubleshooting-Toolkit-and-Cheat-Sheet.md) | [README](./README.md) | [19 - Lab 02: High CPU Spike Isolation](./19-Hands-On-Lab-02-High-CPU-Spike-Isolation.md) |
