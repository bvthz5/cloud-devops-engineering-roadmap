# 10 — Hands-On Practice Labs: Backup & Archiving

---

## Lab 1: Parallel Archival with tar and zstd

```bash
# Create dummy files
mkdir -p /tmp/backup_lab && cd /tmp/backup_lab
for i in {1..20}; do dd if=/dev/urandom of="file_$i.bin" bs=1M count=2 2>/dev/null; done

# Compress with zstd using 4 CPU threads
tar --zstd -cvf archive.tar.zst *.bin

# Verify contents
tar -tvf archive.tar.zst
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
