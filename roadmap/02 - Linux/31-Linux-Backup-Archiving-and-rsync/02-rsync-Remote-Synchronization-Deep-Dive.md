# 02 — rsync Remote Synchronization Deep Dive

`rsync` (Remote Sync) is a fast and versatile file copying tool. It minimizes data transfer across networks using a rolling checksum delta-transfer algorithm, transmitting only the bytes that changed inside modified files.

---

## 1. Essential `rsync` Flags

```bash
rsync -avzP --delete /source/ user@remote:/destination/
```
- **`-a` (Archive Mode):** Equivalent to `-rlptgoD`. Preserves recursion, symlinks, permissions, modification times, owner, and group.
- **`-v` (Verbose):** Detailed output.
- **`-z` (Compress):** Compresses data during transit over network.
- **`-P` (Progress & Partial):** Displays real-time progress and resumes interrupted downloads.
- **`--delete`:** Deletes files in destination that no longer exist in source (creates an exact mirror).
- **`--exclude`:** Excludes patterns (e.g. `--exclude="*.tmp" --exclude="node_modules/"`).
- **`--bwlimit=10M`:** Limits network I/O bandwidth to prevent saturating production links.

---

## 2. The Critical Trailing Slash Nuance

The presence or absence of a trailing slash on the **source** directory completely changes the operation:

```bash
# Case A: WITH trailing slash (Sync contents of source into dest)
rsync -av /var/www/ /backup/
# Result: /backup/index.html, /backup/style.css

# Case B: WITHOUT trailing slash (Sync source directory itself into dest)
rsync -av /var/www /backup/
# Result: /backup/www/index.html, /backup/www/style.css
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Archiving & Compression](./01-Archiving-and-Compression-tar-gzip-bzip2-zstd.md) | [README](./README.md) | [03 - Automated Backups over SSH](./03-Automated-Backups-over-SSH.md) |
