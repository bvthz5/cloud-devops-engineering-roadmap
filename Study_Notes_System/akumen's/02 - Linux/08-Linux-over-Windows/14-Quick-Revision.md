# 14 - Quick Revision Cheat Sheet: Linux over Windows

A 5-minute high-density reference sheet comparing Linux and Windows operating systems for DevOps engineers.

---

## 🚀 Comparison Summary Cheat Sheet

| Metric / Feature | Linux Operating System | Windows Server |
| :--- | :--- | :--- |
| **Licensing** | $0 (Open Source GPL) | Per-core fees + CALs + Cloud surcharges |
| **GUI Overhead** | None (Headless Server default) | Heavy desktop shell overhead |
| **Memory Footprint** | ~100 MB - 500 MB RAM idle | ~1.5 GB - 4.0 GB RAM idle |
| **Case Sensitivity** | **Case-Sensitive** (`File` != `file`) | **Case-Insensitive** (`File` == `file`) |
| **Line Endings** | LF (`\n`) | CRLF (`\r\n`) |
| **Containers** | Native (`cgroups` & `namespaces`) | Hyper-V virtualized layer for Linux containers |
| **Config Model** | Plain-text files under `/etc` | Centralized binary Registry |
| **Reboots** | Zero forced reboots (Live patching) | Frequent required update reboots |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - MCQ](./13-MCQ.md) | [README](./README.md) | [15 - Related Topics](./15-Related-Topics.md) |
