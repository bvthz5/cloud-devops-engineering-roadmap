# 11 - Master Command Flags & Options Reference

This exhaustive cheat sheet summarizes the command-line flags for all core Linux file management tools.

---

## 📋 Comprehensive Flag Matrix

| Command | Flag | Long Option | Function / DevOps Purpose |
|---|:---:|---|---|
| **`ls`** | `-l` | `--long` | Detailed long listing format (permissions, owner, size, time). |
| | `-a` | `--all` | Includes hidden files starting with `.`. |
| | `-h` | `--human-readable` | Formats sizes in KB, MB, GB (use with `-l`). |
| | `-t` | | Sorts entries by modification time, newest first. |
| | `-r` | `--reverse` | Reverses any sorting order. |
| | `-S` | | Sorts entries by file size, largest first. |
| | `-d` | `--directory` | Lists directory entry itself, not its contents. |
| **`mkdir`**| `-p` | `--parents` | Creates missing parent directories; idempotent in scripts. |
| | `-m` | `--mode` | Sets file permissions mode at creation time (`-m 700`). |
| | `-v` | `--verbose` | Prints confirmation message for each directory created. |
| **`rm`** | `-r` | `--recursive` | Recursively deletes directories and their contents. |
| | `-f` | `--force` | Ignores non-existent files; overrides write-protection prompts. |
| | `-i` | `--interactive` | Prompts before deleting every individual file. |
| | `-I` | | Prompts once before deleting >3 files or recursive targets. |
| **`cp`** | `-a` | `--archive` | **Gold Standard:** Preserves permissions, ownership, timestamps, and symlinks recursively. |
| | `-r` | `--recursive` | Recursively copies directory tree. |
| | `-u` | `--update` | Copies only if source is newer than destination. |
| | | `--reflink` | Copy-on-Write instant cloning on XFS/Btrfs. |
| **`mv`** | `-i` | `--interactive` | Prompts before overwriting existing destination file. |
| | `-f` | `--force` | Overwrites destination without confirmation. |
| | `-n` | `--no-clobber` | Never overwrites an existing destination file. |
| | `-b` | `--backup` | Creates backup (`file~`) of destination file before overwriting. |
| **`cat`** | `-n` | `--number` | Numbers all output lines. |
| | `-A` | `--show-all` | Displays invisible characters (tabs as `^I`, endings as `$`). |
| **`tail`** | `-f` | `--follow` | Streams live appending log entries by file descriptor. |
| | `-F` | | Streams live log entries by filename (survives log rotations!). |
| | `-n` | `--lines` | Outputs last N lines (or `-n +K` to skip headers). |
| **`grep`** | `-i` | `--ignore-case` | Performs case-insensitive matching. |
| | `-r` | `--recursive` | Searches all files inside directory tree. |
| | `-n` | `--line-number` | Displays 1-based line numbers of matching lines. |
| | `-v` | `--invert-match` | Inverts matching: outputs lines that do NOT match pattern. |
| **`tar`** | `-c` | `--create` | Creates a new archive file. |
| | `-x` | `--extract` | Extracts files from an archive. |
| | `-z` | `--gzip` | Filters archive through `gzip` compression (`.tar.gz`). |
| | `-j` | `--bzip2` | Filters archive through `bzip2` compression (`.tar.bz2`). |
| | `-J` | `--xz` | Filters archive through `xz` compression (`.tar.xz`). |
| | `-v` | `--verbose` | Verbosely lists files being processed. |
| | `-f` | `--file` | Specifies the archive file name. |
| | `-C` | `--directory` | Changes to target directory before extracting. |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Echo and Redirection](./10-Echo-and-Redirection.md) | [README](./README.md) | [12 - Paths Wildcards and Expansion](./12-Paths-Wildcards-and-Expansion.md) |
