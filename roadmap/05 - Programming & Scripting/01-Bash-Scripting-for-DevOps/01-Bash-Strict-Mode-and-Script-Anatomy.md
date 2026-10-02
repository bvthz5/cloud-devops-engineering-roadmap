# 01 - Bash Strict Mode and Script Anatomy

## 1. The Portable Shebang

Always specify the portable environment shebang rather than hardcoding `/bin/bash`:
```bash
#!/usr/bin/env bash
```
This guarantees compatibility across Linux distributions, macOS, and container base images (Alpine, Ubuntu, Debian).

---

## 2. Unofficial Bash Strict Mode: `set -euo pipefail`

By default, Bash is extraordinarily forgiving: it ignores errors, continues executing after missing files, and masks failed commands inside pipelines. **This causes production data loss!**

Always place this line at the top of every production script:
```bash
set -euo pipefail
IFS=$'\n\t'
```

### Deconstructing the Flags:
- **`-e` (`errexit`):** Exit immediately if any command returns a non-zero exit code.
- **`-u` (`nounset`):** Exit immediately if an undefined/unbound variable is referenced (e.g. prevents catastrophic `rm -rf /tmp/$UNSET_VAR`).
- **`-o pipefail`:** Prevents masked errors in pipelines. Normally, `grep` failing inside `curl ... | grep ... | awk ...` is ignored if the final `awk` succeeds. `pipefail` ensures the entire pipeline fails if *any* stage fails.
- **`IFS=$'\n\t'`:** Sets the Internal Field Separator to newlines and tabs only, preventing bugs when iterating over filenames with spaces.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (04 - Git & Version Control)](../../04%20-%20Git%20%26%20Version%20Control/11-Git-LFS-and-Artifact-Management/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Variables Arrays and Parameter Expansion →](./02-Variables-Arrays-and-Parameter-Expansion.md) |
