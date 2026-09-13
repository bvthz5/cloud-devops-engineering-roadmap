# 03 - File & Directory Deletion: `rm` and Secure Erasing

In Linux, there is no desktop "Recycle Bin" or "Trash Can" for the command line. Deleting a file with `rm` unlinks the inode immediately, making deletion permanent.

---

## 1. `rm` — The Remove Utility

```bash
# Delete a single file:
$ rm document.txt

# Delete multiple files:
$ rm file1.txt file2.txt file3.txt
```

---

## 2. Core Flags & Operational Modes

| Flag | Meaning | Behavior |
|:---:|---|---|
| **`-i`** | Interactive | Prompts for confirmation before deleting every individual file. |
| **`-I`** | Moderate Prompt | Prompts once before deleting more than 3 files or when deleting recursively. |
| **`-f`** | Force | Ignores non-existent files; suppresses confirmation prompts even on write-protected files. |
| **`-r` / `-R`** | Recursive | Traverses and removes directories and their entire contents. |
| **`-v`** | Verbose | Prints the name of each file as it is unlinked. |
| **`--preserve-root`**| Root Guard | Default safeguard preventing execution of `rm -rf /`. |

### Safety-First Interactive Mode:
```bash
$ rm -i important.conf
rm: remove regular file 'important.conf'? y
```

### Recursive Directory Removal:
```bash
# Delete a folder and all nested subfolders and files:
$ rm -rf /tmp/old_build/
```

---

## 3. The Dangerous Scripting Trap: Variable Expansion Bugs

The most catastrophic outages in DevOps history often trace back to unquoted or unvalidated shell variables:

```bash
# DANGEROUS CODE IN A SCRIPT:
TARGET_DIR=""
rm -rf $TARGET_DIR/     # Evaluates to: rm -rf /   <-- WIPES THE SYSTEM!
```

### Safe Production Coding Standards:
```bash
# 1. Parameter Expansion Guard (exits script immediately if variable is null or unset):
rm -rf "${TARGET_DIR:?Target directory variable is not set}/"

# 2. Never run blind wildcards against variables:
# BAD:  rm -rf $APP_DIR/*
# GOOD: [[ -d "$APP_DIR" ]] && rm -rf "${APP_DIR:?}"/*
```

---

## 4. Secure Erasing with `shred`

Standard `rm` only removes the directory entry (unlinks the inode). The physical magnetic or flash storage blocks still contain the original bits until overwritten by future writes!

To securely sanitize sensitive cryptographic keys, passwords, or certificates:

```bash
# Overwrite file with random bytes 3 times, overwrite with zeros, then truncate and delete:
$ shred -u -z -n 3 server.key
```
- **`-n 3`:** Overwrites 3 times with pseudo-random data.
- **`-z`:** Adds a final overwrite with zeros to conceal shredding.
- **`-u`:** Deallocates and deletes the file after overwriting.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Directories mkdir rmdir](./02-Directories-mkdir-rmdir.md) | [README](./README.md) | [04 - Copy cp](./04-Copy-cp.md) |
