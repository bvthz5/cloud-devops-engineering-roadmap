# 07 - Bash Scripting: Real-World Production Scenarios

## Scenario 1: The Accidental `rm -rf /` Disaster

### Incident Summary
A cleanup script ran nightly via cron to clear build directories:
```bash
# OLD DANGEROUS SCRIPT:
rm -rf "$BUILD_DIR/*"
```
During a server upgrade, an environment variable refactoring removed `BUILD_DIR` from `/etc/environment`. When the cron job executed, `$BUILD_DIR` evaluated to an empty string. The shell expanded the command to:
```bash
rm -rf /*
```
The script wiped the entire operating system root partition before the process was killed!

### Production Fix
1. Enable `set -u` (nounset). Any unset variable immediately terminates execution.
2. Use parameter verification:
   ```bash
   rm -rf "${BUILD_DIR:?Error: BUILD_DIR is not set!}"/*
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Subprocesses Redirection and Process Substitution](./06-Subprocesses-Redirection-and-Process-Substitution.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
