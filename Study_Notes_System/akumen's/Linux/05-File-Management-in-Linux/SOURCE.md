# SOURCE: File Management in Linux

This document outlines the origin, foundational inputs, and structural expansions applied to create the **05-File-Management-in-Linux** topic within the **Akumen Study System**.

---

## 📌 Primary Inputs & Scope

The foundation of this topic is built upon core Linux file manipulation utilities and shell concepts:

1. **Navigation & Inspection Utilities:**
   - `pwd`, `cd`, `ls`
   - Path types (Absolute vs Relative), Wildcards (`*`, `?`, `[]`), Globbing.
2. **Directory & File Operations:**
   - `mkdir`, `rmdir`
   - `rm` (Deletion safety, recursive removal)
   - `cp` (Copying, archiving modes, recursive copying)
   - `mv` (Moving, renaming, atomic replacement)
3. **File Content Inspection & Editors:**
   - `cat`, `tac`, `less`, `more`
   - `head`, `tail` (Streaming live logs with `-f`)
   - Text editors: `nano`, `vi`, `vim` (Modes, key bindings, search/replace)
4. **I/O Redirection & Streams:**
   - `echo`
   - Standard input (0), Standard output (1), Standard error (2)
   - Redirection operators: `>`, `>>`, `2>`, `2>&1`, `&>`
   - UNIX Pipes (`|`)

---

## 🛠️ Educational Expansions & DevOps Extensions

To transform basic command syntax into a production-grade DevOps study guide, the original prompt material was systematically expanded to include:

- **Command Options vs In-Editor Commands:** Clear distinction between CLI flags (e.g., `vim -R`) and internal interactive commands (e.g., `:wq`).
- **File Metadata & Inodes:** Deep dive into `stat`, hard links (`ln`), soft/symbolic links (`ln -s`), and inode pointer mechanics.
- **Permissions & Security:** Comprehensive octal/symbolic representation (`chmod`, `chown`, `umask`, SUID/SGID/Sticky Bit).
- **Search & Discovery:** High-performance searching with `find`, `grep`, and `file`.
- **Archiving & Compression:** Enterprise backup operations with `tar`, `gzip`, `bzip2`, and `xz`.
- **Production Scenarios & Incident Response:** Real-world playbooks for disk space exhaustion, log rotation, lock file remediation, and broken symlinks.
- **Assessment Suite:** Technical interview preparation, hands-on terminal exercises, multiple-choice questions, and quick revision cheat sheets.

---

## 📂 Topic File Index

- [`README.md`](./README.md)
- [`01-Navigation-ls-cd-pwd.md`](./01-Navigation-ls-cd-pwd.md)
- [`02-Directories-mkdir-rmdir.md`](./02-Directories-mkdir-rmdir.md)
- [`03-Delete-rm.md`](./03-Delete-rm.md)
- [`04-Copy-cp.md`](./04-Copy-cp.md)
- [`05-Move-and-Rename-mv.md`](./05-Move-and-Rename-mv.md)
- [`06-Viewing-cat-tac-less-more.md`](./06-Viewing-cat-tac-less-more.md)
- [`07-Head-and-Tail.md`](./07-Head-and-Tail.md)
- [`08-Nano.md`](./08-Nano.md)
- [`09-Vi-and-Vim.md`](./09-Vi-and-Vim.md)
- [`10-Echo-and-Redirection.md`](./10-Echo-and-Redirection.md)
- [`11-All-Flags-Cheat-Sheet.md`](./11-All-Flags-Cheat-Sheet.md)
- [`12-Paths-Wildcards-and-Expansion.md`](./12-Paths-Wildcards-and-Expansion.md)
- [`13-Permissions-and-Ownership.md`](./13-Permissions-and-Ownership.md)
- [`14-Links-and-Metadata.md`](./14-Links-and-Metadata.md)
- [`15-Find-Search-and-Inspection.md`](./15-Find-Search-and-Inspection.md)
- [`16-Archives-and-Compression.md`](./16-Archives-and-Compression.md)
- [`17-Real-World-Scenarios.md`](./17-Real-World-Scenarios.md)
- [`18-Troubleshooting.md`](./18-Troubleshooting.md)
- [`19-Interview-QA.md`](./19-Interview-QA.md)
- [`20-Hands-On-Practice.md`](./20-Hands-On-Practice.md)
- [`21-MCQ.md`](./21-MCQ.md)
- [`22-Quick-Revision.md`](./22-Quick-Revision.md)
- [`23-Related-Topics.md`](./23-Related-Topics.md)
---

| Back to Index |
| :---: |
| [README](./README.md) |
