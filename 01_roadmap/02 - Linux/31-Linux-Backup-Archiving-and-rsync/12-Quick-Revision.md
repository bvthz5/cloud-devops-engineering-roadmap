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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (32-SSH-Architecture-Key-Management-and-Tunneling) →](../32-SSH-Architecture-Key-Management-and-Tunneling/01-SSH-Protocol-Architecture-and-Cryptography.md) |
