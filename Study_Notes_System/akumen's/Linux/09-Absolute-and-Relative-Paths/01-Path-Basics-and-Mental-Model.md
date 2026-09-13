# 01 - Path Basics & Mental Model

A **Path** is an address string that specifies the unique location of a file or directory within the Linux single-tree filesystem hierarchy.

---

## 🌲 The Single Tree Hierarchy

In Linux, all files, directories, physical hard drives, mounted network volumes, and virtual device interfaces live under a single root directory denoted by a forward slash (`/`).

```text
/ (Root)
├── bin
├── etc/
│   └── nginx/
│       └── nginx.conf
├── home/
│   └── alice/
│       └── document.txt
└── var/
    └── log/
        └── syslog
```

Forward slashes (`/`) serve two roles in Linux paths:
1. **The Initial Slash:** Represents the top-level **Root Directory** (e.g., `/etc`).
2. **Separator Slashes:** Separate subdirectories in a directory path (e.g., `/home/alice/document.txt`).

---

## 📍 Current Working Directory (`pwd`)

Every running process in Linux operates within a **Current Working Directory (CWD)**. When you open a terminal shell, your CWD is initially set to your user's home directory (e.g., `/home/ubuntu` or `/root`).

```bash
# Print current working directory
pwd
# Output: /home/ubuntu
```

Path resolution depends heavily on your current position in the tree:
- If a path **starts with `/`**, the OS resolves it starting from the Root directory.
- If a path **does NOT start with `/`**, the OS resolves it starting from your Current Working Directory (`pwd`).
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Absolute Paths](./02-Absolute-Paths.md) |
