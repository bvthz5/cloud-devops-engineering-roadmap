# 09 - The Vi & Vim Modal Text Editor

**Vim (Vi IMproved)** is a ubiquitous, highly efficient modal text editor present on virtually every Unix/Linux system, server, and container environment.

---

## 1. The Core Philosophy: Modal Editing

Unlike modeless editors (like Notepad or Nano) where every keypress directly inserts text, Vim is **modal**. The same key performs different actions depending on your active **Mode**.

```
                           ┌─────────────────────────┐
                   ┌──────►│   NORMAL MODE (ESC)     ├──────┐
                   │       │ Navigation, Deletion,   │      │
                   │       │ Copying, Pasting        │      │
                   │       └────────────▲────────────┘      │
                   │                    │                   │
      Press 'i',   │                    │ Press 'ESC'       │ Press ':',
      'a', or 'o'  │                    │                   │ '/', or '?'
                   │       ┌────────────┴────────────┐      │
                   │       │   INSERT MODE (i)       │      │
                   ├───────┤ Directly typing text    │      │
                   │       │ into the file buffer    │      │
                   │       └─────────────────────────┘      │
                   │                                        │
                   │       ┌─────────────────────────┐      │
                   │       │   VISUAL MODE (v / V)   │      │
                   ├───────┤ Highlighting text blocks│      │
                   │       │ for batch operations    │      │
                   │       └─────────────────────────┘      │
                   │                                        │
                   │       ┌─────────────────────────┐      │
                   └───────┤  COMMAND-LINE MODE (:)  │◄─────┘
                           │ Saving (:w), Quitting  │
                           │ (:q), Searching (/)    │
                           └─────────────────────────┘
```

---

## 2. Command-Line Options (Starting Vim)

These options are passed from the terminal when launching Vim:

| Command | Action |
|---|---|
| **`vim filename`** | Opens or creates `filename` in Normal mode. |
| **`vim +150 filename`** | Opens the file and jumps directly to line 150. |
| **`vim +/pattern filename`**| Opens the file and jumps to the first occurrence of `pattern`. |
| **`vim -R filename`** | Opens in Read-Only mode. |
| **`vim -d file1 file2`** | Opens both files side-by-side in `vimdiff` mode to compare changes. |

---

## 3. In-Editor Modes & Hotkeys

### Entering Insert Mode (from Normal Mode):
- **`i`**: Insert before current cursor position.
- **`I`**: Insert at the beginning of the current line.
- **`a`**: Append after current cursor position.
- **`A`**: Append at the end of the current line.
- **`o`**: Open a new blank line *below* the current line and enter Insert mode.
- **`O`**: Open a new blank line *above* the current line and enter Insert mode.
- **`Esc`**: Return to Normal Mode.

---

### Navigation Motions (Normal Mode):
- **`h`**, **`j`**, **`k`**, **`l`**: Left, Down, Up, Right (keeps fingers on home row).
- **`w`**: Jump forward to the start of the next word.
- **`b`**: Jump backward to the start of the previous word.
- **`0`** (Zero): Jump to the start of the current line.
- **`$`**: Jump to the end of the current line.
- **`gg`**: Jump to the very first line of the file.
- **`G`**: Jump to the very last line of the file.
- **`:N`** or **`NG`**: Jump directly to line number N (e.g. `45G` jumps to line 45).

---

### Editing, Copying & Deleting (Normal Mode):
- **`x`**: Delete character under the cursor.
- **`dd`**: Delete (cut) the current line.
- **`5dd`**: Delete (cut) 5 lines starting from cursor.
- **`dw`**: Delete from cursor to the end of the word.
- **`yy`**: Yank (copy) the current line.
- **`p`**: Paste clipboard buffer *after* the cursor line.
- **`P`**: Paste clipboard buffer *before* the cursor line.
- **`u`**: Undo last action.
- **`Ctrl + r`**: Redo last undone action.

---

### Command-Line Mode (Press `:` in Normal Mode):
- **`:w`**: Write (save) the file to disk.
- **`:w!`**: Force save (if file is write-protected by owner).
- **`:q`**: Quit Vim (fails if unsaved changes exist).
- **`:q!`**: Force quit without saving changes.
- **`:wq`** or **`:x`** or **`ZZ`**: Save changes and exit.
- **`:%s/old/new/g`**: Replace all occurrences of `old` with `new` across the whole file.
- **`:set nu`**: Enable line numbers.
- **`:set nonu`**: Disable line numbers.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Nano](./08-Nano.md) | [README](./README.md) | [10 - Echo and Redirection](./10-Echo-and-Redirection.md) |
