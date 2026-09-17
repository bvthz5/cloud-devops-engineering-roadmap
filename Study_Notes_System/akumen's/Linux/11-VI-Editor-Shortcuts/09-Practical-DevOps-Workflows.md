# 09 - Practical DevOps Workflows

These are the most common real-world VI editing patterns encountered daily by system administrators, SREs, and DevOps engineers working on production Linux servers.

---

## 🏭 Scenario 1: Editing NGINX Config File Safely

```text
┌─────────────────────────────────────────────────────────────┐
│                    DevOps Workflow Pattern                   │
│           Edit → Validate → Reload → Verify Logs            │
└─────────────────────────────────────────────────────────────┘
```

### Steps
1. Open NGINX config file as a standard user (no sudo needed to open):
   ```bash
   vi /etc/nginx/sites-available/myapp.conf
   ```
2. Navigate to `server_name` directive with `/server_name` search.
3. Press `n` to find occurrences, use `cw` to change hostname inline.
4. You forgot `sudo`? Use the rescue save instead of quitting:
   ```text
   :w !sudo tee %
   ```
5. Execute config validation and reload **without leaving Vim**:
   ```text
   :!sudo nginx -t
   :!sudo systemctl reload nginx
   ```

---

## 🏭 Scenario 2: Block-Comment Multiple Lines in a Bash Script (Visual Block Mode)

Adding `#` to comment out lines 10–25 in a shell script:

1. Move cursor to **line 10** using `:10`.
2. Press `Ctrl+V` to enter **Visual Block Mode**.
3. Press `25G` to extend selection down to line 25.
4. Press `I` (capital i — Insert at start of block).
5. Type `# ` (hash + space).
6. Press `<ESC>`. All 16 lines now have `# ` prepended.

---

## 🏭 Scenario 3: Editing a YAML Configuration File with Correct Indentation

YAML is whitespace-sensitive. Misconfigured tab/space expansion breaks Kubernetes manifests and Ansible playbooks.

### Set YAML-safe editor settings before editing:
```text
:set expandtab       " Convert Tab keypresses to spaces (never insert literal tab)
:set tabstop=2       " Display tab characters as 2 spaces wide
:set shiftwidth=2    " Set indentation level for >> and << to 2 spaces
:set autoindent      " Automatically indent new lines to match previous line
```

### Optional: Persist in `~/.vimrc`:
```bash
echo "autocmd FileType yaml setlocal expandtab tabstop=2 shiftwidth=2 autoindent" >> ~/.vimrc
```

---

## 🏭 Scenario 4: Emergency Server Rescue — Edit `/etc/sudoers` Safely

> ⚠️ Always prefer `visudo` for sudoers! In edge cases where `visudo` is unavailable:

```bash
EDITOR=vim sudo -e /etc/sudoers
```

Or using Vim directly with syntax checking:
```bash
sudo vim /etc/sudoers
```
After finishing edits inside Vim:
* `:w` — Write.
* `:q` — Quit.
* **Immediately test** with `sudo -l -U username` **before closing your session!**

---

## 🏭 Scenario 5: Find & Replace Environment Variables Across a Config File

Replacing `DB_HOST=old-db.internal` with `DB_HOST=db.production.internal` globally:

```text
:%s/old-db\.internal/db.production.internal/gc
```
The `c` flag prompts for confirmation before each replacement — safe for production changes!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Multiple Files and Splits](./08-Multiple-Files-and-Splits.md) | [README](./README.md) | [10 - Troubleshooting](./10-Troubleshooting.md) |
