# Linux File Permissions & Ownership

## Purpose
Learn the Linux permission model to secure files, enable collaboration, and troubleshoot access errors.

```text
The Triad
User (u) | Group (g) | Others (o)
Read (r) | Write (w) | Execute (x)
chmod | chown | chgrp
```

## Source Foundation

The supplied material covers the permission model, the differences between file and directory permissions, inspecting permissions with `ls -l` and `stat`, modifying permissions with `chmod` (symbolic and octal), managing ownership with `chown`, and special permissions like SUID, SGID, and the Sticky Bit.

The original source concepts have been expanded and organized into this structured study module with clearly identified learning expansions, scenarios, troubleshooting guides, and practice labs.

## Rule of thumb
*   Need to change who owns it? → `chown`
*   Need to change what they can do? → `chmod`
*   Need to fix "Permission denied"? → Check the directory path with `namei`

## Learning path
*   Permission fundamentals
*   Read/write/execute — files vs directories
*   Ownership and groups
*   ls, namei, stat
*   chmod symbolic notation
*   chmod octal notation
*   setuid, setgid, sticky bit
*   chown and chgrp
*   Real-world DevOps scenarios
*   Permission troubleshooting
*   Interview Q&A
*   Hands-on terminal practice
*   MCQs
*   Quick revision
*   Related topics
