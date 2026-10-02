# 07 - File Operations, Save, and Quit

All file-level operations in VI are executed in **Command-Line Mode** (entered by pressing `:` from Normal Mode).

---

## 💾 Save & Quit Reference Table

| Command | Action |
| :---: | :--- |
| `:w` | **Write (save)** file to disk without quitting |
| `:w filename.txt` | Write current buffer to a **new file** named `filename.txt` |
| `:q` | **Quit** VI (only succeeds if no unsaved changes exist) |
| `:q!` | **Force quit** discarding all unsaved changes |
| `:wq` | **Write (save) and quit** in one operation |
| `:wq!` | Force **write and quit** (overrides file read-only flag if user has OS write permissions) |
| `ZZ` | Normal-Mode shortcut — **save and quit** (equivalent to `:wq`) |
| `ZQ` | Normal-Mode shortcut — **quit without saving** (equivalent to `:q!`) |
| `:x` | Save **only if changes were made**, then quit (slightly different from `:wq` which always writes) |

---

## 📂 Reading and Inserting External File Content

| Command | Action |
| :---: | :--- |
| `:r filename` | **Read** file `filename` and insert its contents below current cursor line |
| `:r !command` | **Execute shell command** and insert its output into the file below the cursor |

### Examples
```text
:r /etc/nginx/nginx.conf.bak     " Insert backup config below current line
:r !date                          " Insert current system date output
:r !cat /etc/hosts                " Insert full /etc/hosts content below cursor
```

---

## 🐚 Executing Shell Commands Without Leaving VI

| Command | Action |
| :---: | :--- |
| `:!command` | Run `command` in shell and **display output** (returns to Vim after pressing Enter) |
| `:!ls -la` | Example: list directory contents without exiting |
| `:!systemctl reload nginx` | Example: reload nginx after editing its config |

---

## 🚑 Saving a File You Opened Without `sudo` (Read-Only)

This is one of the most essential DevOps rescue commands in VI:

```text
:w !sudo tee %
```

### Breakdown:
* `:w` — Write the current buffer's content.
* `!sudo tee` — Pipe the output through `sudo tee` (a command that writes stdin to a file with root privileges).
* `%` — VI special symbol representing the **current filename**.

This writes the file to disk with root privileges without needing to close and re-open.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Search and Replace](./06-Search-and-Replace.md) | [README](./README.md) | [08 - Multiple Files and Splits](./08-Multiple-Files-and-Splits.md) |
