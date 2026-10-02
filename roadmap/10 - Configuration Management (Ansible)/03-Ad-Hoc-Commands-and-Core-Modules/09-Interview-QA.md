# 09 - Interview Q&A: Ad-Hoc Commands & Modules

### Q: Why does `command` module fail when using pipes `|`?
**Answer:** The `command` module executes binaries directly via execve without invoking a shell process (`/bin/sh`). Pipes, redirects (`>`), and environment variables require the `shell` module.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
