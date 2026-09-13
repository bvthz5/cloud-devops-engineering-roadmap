# 16 - Archives & Compression: `tar`, `gzip`, `bzip2`, and `xz`

Archiving packages multiple files into a single bundle, while compression applies mathematical algorithms to reduce storage consumption.

---

## 1. Archiving vs. Compression

```
┌─────────────────────────────────────────────────────────────┐
│ 1. Archiving (`tar`):                                       │
│ • Binds thousands of files into 1 single file (.tar).       │
│ • File size remains the same (sum of all files).            │
├─────────────────────────────────────────────────────────────┤
│ 2. Compression (`gzip` / `bzip2` / `xz`):                  │
│ • Compresses a SINGLE file to reduce byte size.             │
├─────────────────────────────────────────────────────────────┤
│ 3. Combined Archive + Compression (.tar.gz / .tar.xz):      │
│ • Bundles files into a tarball, then compresses the stream. │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. The `tar` Utility (Tape Archive)

`tar` is the standard Linux archiving utility.

### The 4 Core Mode Flags (Must Pick 1):
- **`-c`**: Create a new archive.
- **`-x`**: Extract an existing archive.
- **`-t`**: List (table of contents) of an archive without extracting.
- **`-r`**: Append files to the end of an existing archive.

### Compression Filter Flags:
- **`-z`**: Filter through **`gzip`** (`.tar.gz` or `.tgz`). Fast, moderate compression.
- **`-j`**: Filter through **`bzip2`** (`.tar.bz2`). Slower, better compression.
- **`-J`**: Filter through **`xz`** (`.tar.xz`). Slowest, maximum compression ratio (standard for Linux kernel releases).

---

## 3. High-Power `tar` Recipes

```bash
# 1. Create a gzipped tarball of a directory:
$ tar -czvf app_backup.tar.gz /var/www/html/

# 2. Extract a gzipped tarball into a specific target directory (-C):
$ sudo tar -xzvf app_backup.tar.gz -C /opt/restored_app/

# 3. View archive contents without extracting:
$ tar -tzvf app_backup.tar.gz

# 4. Create an ultra-compressed xz archive for release:
$ tar -CJvf release-v1.0.0.tar.xz ./build/
```

---

## 4. Standalone Compression Tools (`gzip`, `bzip2`, `xz`, `zip`)

```bash
# 1. gzip / gunzip:
$ gzip logfile.log       # Creates logfile.log.gz AND REMOVES original logfile.log!
$ gunzip logfile.log.gz  # Decompresses logfile.log.gz back to logfile.log

# Compress while keeping original file (-k):
$ gzip -k logfile.log

# 2. zip / unzip (for Windows compatibility):
$ zip -r archive.zip folder/
$ unzip archive.zip -d /target/path/
```

### Compression Ratio Comparison:

| Tool | Speed | Compression Ratio | Common Extension |
|---|:---:|:---:|:---:|
| **`gzip`** | Fast | Good | `.gz` / `.tar.gz` |
| **`bzip2`** | Moderate | Better | `.bz2` / `.tar.bz2` |
| **`xz`** | Slower | **Best** | `.xz` / `.tar.xz` |
| **`zip`** | Fast | Good (Cross-Platform) | `.zip` |
