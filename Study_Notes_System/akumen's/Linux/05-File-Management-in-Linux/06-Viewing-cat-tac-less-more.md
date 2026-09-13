# 06 - Viewing File Contents: `cat`, `tac`, `less`, and `more`

Linux provides multiple command-line utilities for inspecting, streaming, and paginating text file contents.

---

## 1. `cat` — Concatenate & Print

`cat` reads files sequentially and writes them to standard output.

```bash
# Print a file to terminal:
$ cat /etc/hosts

# Concatenate multiple files into one:
$ cat part1.txt part2.txt part3.txt > complete.txt
```

### High-Frequency Flags:
- **`-n`:** Number all output lines starting from 1.
- **`-b`:** Number non-empty lines only.
- **`-s`:** Squeeze multiple adjacent blank lines into a single blank line.
- **`-A` (or `-vET`):** Displays invisible characters: tabs appear as **`^I`** and line endings appear as **`$`**. (Crucial for debugging Python indent errors or Windows `\r\n` line endings!).

```bash
$ cat -A script.sh
#!/bin/bash$
VAR="value"^I# Tab indented$
```

> [!CAUTION]
> **The Anti-Pattern: Never run `cat` on a massive file!**
> Running `cat massive_production.log` on a 20 GB file will overwhelm your terminal emulator, flood memory buffers, and freeze your SSH session. Use **`less`** instead!

---

## 2. `tac` — Reverse Cat (Bottom to Top)

`tac` is literally `cat` spelled backwards. It outputs the file starting from the **last line and working upward to the first line**.

```bash
$ tac /var/log/syslog | head -n 20
# (Instantly inspects the most recent 20 lines without reading top-down!)
```

---

## 3. `less` — The Gold Standard Interactive Pager

The Unix adage states: *"less is more, more or less."*
Unlike `cat`, **`less` does not read the entire file into memory before opening**. It lazily streams only the visible terminal screen, opening a 50 GB log file in less than 1 millisecond.

```bash
$ less /var/log/syslog
```

### Essential `less` Navigation Hotkeys:

| Key Binding | Action |
|:---:|---|
| **`Space`** or **`f`** | Scroll forward one full page. |
| **`b`** | Scroll backward one full page. |
| **`j`** or **`Down Arrow`** | Scroll down one single line. |
| **`k`** or **`Up Arrow`** | Scroll up one single line. |
| **`G`** | Jump directly to the **end of the file** (bottom). |
| **`g`** or **`1G`** | Jump directly to the **beginning of the file** (top). |
| **`/pattern`** | Search forward for `pattern` (press `Enter`). |
| **`?pattern`** | Search backward for `pattern`. |
| **`n`** | Jump to **next** search match. |
| **`N`** | Jump to **previous** search match. |
| **`F`** | Follow mode (behaves like `tail -f`; press `Ctrl+C` to return to normal less mode). |
| **`q`** | **Quit** and return to the shell prompt. |

---

## 4. `more` — The Legacy Pager

`more` is the historic Unix pager that preceded `less`.
- It only allows forward scrolling (you cannot scroll backward in older implementations).
- Kept in modern systems primarily for backwards compatibility with legacy scripts.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Move and Rename mv](./05-Move-and-Rename-mv.md) | [README](./README.md) | [07 - Head and Tail](./07-Head-and-Tail.md) |
