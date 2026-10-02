# 12 — Quick Revision Cheat Sheet: Backup & rsync

---

## 1. Command Reference

```bash
# Tar & zstd
tar --zstd -cvf backup.tar.zst /data   # Compress
tar --zstd -xvf backup.tar.zst -C /out  # Extract

# rsync Mirroring
rsync -avzP --delete /src/ /dest/      # Incremental mirror
rsync -avzPn --delete /src/ /dest/     # Dry-run preview
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple Choice Questions](./11-MCQ.md) | [README](./README.md) | [Next Module: 32 - SSH Architecture](../32-SSH-Architecture-Key-Management-and-Tunneling/README.md) |
