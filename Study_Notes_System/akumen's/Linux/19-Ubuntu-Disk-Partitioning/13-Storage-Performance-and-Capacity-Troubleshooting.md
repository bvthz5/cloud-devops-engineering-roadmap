# 13. Storage Performance & Capacity Troubleshooting

## 1. Storage Capacity Auditing

```bash
# Display disk usage by mounted filesystems in human-readable format
df -h

# Check inode usage (prevents "No space left on device" when disk space is free)
df -i

# Find top 10 largest directories
sudo du -ahx /var | sort -rh | head -n 10
```

## 2. Storage Performance Benchmarking (`sysstat` & `fio`)

```bash
# Monitor disk I/O stats in real time (2 second interval)
iostat -xz 2

# Identify processes causing high disk write I/O
sudo iotop -o
```

## 3. Resolving "No space left on device" Errors
If `df -h` shows space available but files cannot be written:
1. **Exhausted Inodes:** Check `df -i`. Delete directories with millions of tiny files (e.g., mail queues/session files).
2. **Deleted Files Held Open by Processes:** Process holds file handle open after `rm`. Check with `sudo lsof +L1` and restart responsible service.
