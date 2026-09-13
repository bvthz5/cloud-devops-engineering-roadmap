# File Management in Linux: Core Commands, Text Manipulation & Administration

Welcome to the definitive hands-on guide on **File Management in Linux**. This module covers the foundational command-line utilities, text viewers, editors, redirection streams, search tools, archiving, and permissions required for Linux systems administration, cloud engineering, and DevOps automation.

---

## 🗺️ File Management Mastery Map

```
                             FILE MANAGEMENT IN LINUX
                                       │
         ┌──────────────┬──────────────┼──────────────┬──────────────┐
         ▼              ▼              ▼              ▼              ▼
   [ Navigation ] [ Operations ]   [ Viewing ]    [ Editing ]   [ Streams & I/O ]
   • pwd, cd, ls  • mkdir, rmdir   • cat, tac     • nano (nano) • echo, printf
   • realpath     • cp, mv, rm     • less, more   • vi / vim    • >, >>, <, 2>&1
   • pushd, popd  • shred, unlink  • head, tail   • modes/cmd   • pipes (|), tee
         │              │              │              │              │
         └──────────────┴──────────────┼──────────────┴──────────────┘
                                       │
         ┌──────────────┬──────────────┴──────────────┬──────────────┐
         ▼              ▼                             ▼              ▼
   [ Metadata ]   [ Search & Find ]            [ Compression ] [ Advanced ]
   • chmod, chown • find, grep, file           • tar, gzip     • Wildcards, globbing
   • ln, stat     • which, whereis             • bzip2, xz     • SUID, SGID, Sticky
```

---

## 🎯 Module Learning Objectives

1. **Precision Navigation:** Master directory tree movement using `pwd`, `cd`, `ls`, relative vs. absolute paths, and directory stacks (`pushd`/`popd`).
2. **Safe Creation & Deletion:** Learn the safest practices for `mkdir -p`, `rm -rf`, preserving timestamps with `cp -a`, and atomic moves with `mv`.
3. **High-Performance Viewing:** Know when to use `cat` vs. pagination with `less`, streaming log monitoring with `tail -f`, and reverse inspection with `tac`.
4. **Terminal Text Editing:** Master both novice-friendly editing (`nano`) and professional modal text manipulation in `vim` (Normal, Insert, Visual, Command modes).
5. **I/O Redirection & Pipelines:** Direct standard output (`stdout`), standard error (`stderr`), append streams, discard to `/dev/null`, and pipe into `tee`.
6. **Search & Archiving:** Locate files by size, date, and permissions with `find`, extract archives with `tar`, and compress data with `gzip`/`xz`.
7. **Production DevOps Scenarios:** Handle runaway log truncations, accidental `rm` recoveries, mass file permissions hardening, and script automation.

---

## 📋 Module Contents

| File | Title | Key Topics Covered |
|---|---|---|
| [01-Navigation-ls-cd-pwd.md](./01-Navigation-ls-cd-pwd.md) | Navigation Essentials | `pwd`, `cd`, `ls -lah`, flags, shortcuts (`~`, `-`, `.`, `..`) |
| [02-Directories-mkdir-rmdir.md](./02-Directories-mkdir-rmdir.md) | Directory Operations | `mkdir -p -m`, `rmdir`, creating nested tree structures |
| [03-Delete-rm.md](./03-Delete-rm.md) | Deletion Mechanics | `rm -rf -i`, unlinking, `shred`, accidental deletion prevention |
| [04-Copy-cp.md](./04-Copy-cp.md) | Copying Files & Trees | `cp -r`, archive mode `cp -a`, preserving attributes, reflink |
| [05-Move-and-Rename-mv.md](./05-Move-and-Rename-mv.md) | Move & Atomic Rename | `mv -i -u -b`, cross-filesystem moves vs atomic same-disk rename |
| [06-Viewing-cat-tac-less-more.md](./06-Viewing-cat-tac-less-more.md) | File Viewers | `cat -n`, `tac`, `less` navigation shortcuts, `more` differences |
| [07-Head-and-Tail.md](./07-Head-and-Tail.md) | Head & Tail Utilities | `head -n`, `tail -n`, real-time monitoring `tail -f` and `tail -F` |
| [08-Nano.md](./08-Nano.md) | Nano Text Editor | Nano shortcuts (`Ctrl+O`, `Ctrl+X`, `Ctrl+W`), `.nanorc` configs |
| [09-Vi-and-Vim.md](./09-Vi-and-Vim.md) | Vi / Vim Mastery | Modal editing (Normal, Insert, Visual, Command), motions, `:wq` |
| [10-Echo-and-Redirection.md](./10-Echo-and-Redirection.md) | I/O Streams & Redirection | `stdout`, `stderr`, `>`, `>>`, `2>`, `2>&1`, pipes (`\|`), `tee -a` |
| [11-All-Flags-Cheat-Sheet.md](./11-All-Flags-Cheat-Sheet.md) | Master Command Flags | Exhaustive flag reference for every core file utility |
| [12-Paths-Wildcards-and-Expansion.md](./12-Paths-Wildcards-and-Expansion.md) | Globbing & Expansion | Wildcards (`*`, `?`, `[]`), brace expansion (`{a,b}`), escaping |
| [13-Permissions-and-Ownership.md](./13-Permissions-and-Ownership.md) | Permissions & Ownership | `chmod`, `chown`, `chgrp`, `umask`, SUID, SGID, Sticky Bit |
| [14-Links-and-Metadata.md](./14-Links-and-Metadata.md) | Links & Inode Metadata | Hard links vs Symbolic links (`ln -s`), `stat`, timestamps |
| [15-Find-Search-and-Inspection.md](./15-Find-Search-and-Inspection.md) | Search & File Inspection | `find` recipes, `grep -rni`, magic byte inspection with `file` |
| [16-Archives-and-Compression.md](./16-Archives-and-Compression.md) | Archiving & Compression | `tar -czvf`, `tar -xvf`, `gzip`, `bzip2`, `xz`, `zip`/`unzip` |
| [17-Real-World-Scenarios.md](./17-Real-World-Scenarios.md) | Production Scenarios | Runaway log zeroing, logrotate, CI/CD pipeline artifact storage |
| [18-Troubleshooting.md](./18-Troubleshooting.md) | Diagnostic Playbooks | Permission denied, argument list too long, device or resource busy |
| [19-Interview-Q&A.md](./19-Interview-Q&A.md) | Technical Interview QA | 10 In-depth file management questions with interview frameworks |
| [20-Hands-On-Practice.md](./20-Hands-On-Practice.md) | Terminal Exercises | 5 Hands-on labs (redirection, safe deletion, vim workout, links) |
| [21-MCQ.md](./21-MCQ.md) | Self-Assessment Quiz | 10 Multiple-choice questions with answer keys & justifications |
| [22-Quick-Revision.md](./22-Quick-Revision.md) | 5-Minute Cheat Sheet | High-speed command reference, vim survival guide, syntax tables |
| [23-Related-Topics.md](./23-Related-Topics.md) | Downstream Connections | Bash scripting, sed & awk, cron automation, GitOps |
| [SOURCE.md](./SOURCE.md) | Source Material Mapping | Mapping of notes to the original file management command set |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| — | You are here | [01 - Navigation ls cd pwd](./01-Navigation-ls-cd-pwd.md) |
