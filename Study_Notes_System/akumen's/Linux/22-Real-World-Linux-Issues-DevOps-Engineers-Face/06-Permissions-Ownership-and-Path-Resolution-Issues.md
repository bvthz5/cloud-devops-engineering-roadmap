# 06 — Permissions, Ownership, and PATH Resolution Issues

## 1. Scenario: Permission Denied & PATH Resolution Failures

```text
Scenario
   ↓
Symptoms: "Permission denied", "command not found", or deployment script fails to execute binary
   ↓
What could cause it? Incorrect chmod/chown bits, script lacking execution bit (+x), binary missing from $PATH
   ↓
Diagnostic commands: ls -la, namei -l /path/to/file, echo $PATH, type -a command, which command
   ↓
Fix / mitigate: Grant appropriate permissions (chmod +x), fix file ownership (chown), export PATH
```

## 2. Directory Traversal Permission Requirements
To access a file inside a directory hierarchy (e.g. `/var/www/html/index.html`), the executing user MUST have **execute (`x`) permission on every parent directory** in the path!

```bash
# Debug complete path directory permission chain
namei -l /var/www/html/index.html
```

### Sample `namei -l` Output
```text
f: /var/www/html/index.html
drwxr-xr-x root root /
drwxr-xr-x root root var
drwxr-x--- root root www      <-- Missing 'x' for others!
drwxr-xr-x root root html
-rw-r--r-- root root index.html
```
> Fix: `chmod o+x /var/www`

## 3. Resolving `command not found` ($PATH Debugging)

```bash
# Print current session PATH directories
echo "$PATH"

# Locate all instances of command binary
type -a node
which node
```

### Fixing `$PATH` inside Non-Interactive Scripts & Cron
```bash
# Explicitly set PATH at script header:
export PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:$PATH"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Inode Exhaustion & I/O](./05-Inode-Exhaustion-and-Disk-IO-Bottlenecks.md) | [README](./README.md) | [07 - Windows CRLF Bugs](./07-Windows-CRLF-Line-Ending-Problems.md) |
