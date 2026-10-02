# 08 - Multiple Files and Window Splits

In production environments, system administrators and DevOps engineers routinely need to compare and edit multiple configuration files simultaneously. VI supports multi-file buffers and split windows natively.

---

## 📁 Opening Multiple Files

```bash
# Open two files at once in separate buffers
vi file1.conf file2.conf

# Open files in vertical side-by-side split view
vim -O file1.conf file2.conf

# Open files in horizontal stacked split view
vim -o file1.conf file2.conf
```

---

## 🗂️ Buffer Navigation (Multiple Files)

When multiple files are loaded into Vim as buffers, navigate between them using:

| Command | Action |
| :---: | :--- |
| `:ls` *or* `:buffers` | **List** all open buffers and their status |
| `:bn` *or* `:bnext` | Switch to **next** buffer |
| `:bp` *or* `:bprev` | Switch to **previous** buffer |
| `:b3` | Switch directly to **buffer number 3** |
| `:b filename` | Switch to buffer by **partial filename match** |
| `:n` | Move to **next file** in the argument list |
| `:prev` | Move to **previous file** in the argument list |

---

## 🪟 Window Splits (Panes)

VI supports splitting the active window to display multiple files (or multiple views of the same file) simultaneously.

### Horizontal Split (`:sp`)
Divides the window into top and bottom panes:
```text
:sp filename.conf         " Open filename.conf in horizontal split above
:sp                       " Duplicate current buffer in a horizontal split
```

### Vertical Split (`:vsp`)
Divides the window into left and right side-by-side panes:
```text
:vsp filename.conf        " Open filename.conf in vertical split to the right
:vsp                      " Duplicate current buffer in a vertical split
```

---

## 🕹️ Navigating Between Split Windows

After splitting, move focus between panes using `Ctrl+W` prefix key combinations:

| Shortcut | Action |
| :---: | :--- |
| `Ctrl+W h` | Move focus to **Left** pane |
| `Ctrl+W j` | Move focus to **Below** pane |
| `Ctrl+W k` | Move focus to **Above** pane |
| `Ctrl+W l` | Move focus to **Right** pane |
| `Ctrl+W w` | Cycle focus through **next** window |
| `Ctrl+W =` | Equalize all window **sizes** |
| `Ctrl+W +` | Increase current window **height** |
| `Ctrl+W -` | Decrease current window **height** |
| `Ctrl+W >` | Increase current window **width** |
| `Ctrl+W <` | Decrease current window **width** |

### Closing Split Windows
| Command | Action |
| :---: | :--- |
| `:q` | Close **current** focused window pane |
| `:only` | Close **all other panes**, keeping only the currently focused one |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - File Operations Save and Quit](./07-File-Operations-Save-and-Quit.md) | [README](./README.md) | [09 - Practical DevOps Workflows](./09-Practical-DevOps-Workflows.md) |
