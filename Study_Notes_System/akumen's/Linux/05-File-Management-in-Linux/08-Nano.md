# 08 - The Nano Terminal Text Editor

**GNU nano** is a lightweight, modeless command-line text editor designed to be simple, predictable, and immediately accessible without memorizing modal commands.

---

## 1. Starting Nano

```bash
# Open an existing file or create a new one:
$ nano /etc/hosts

# Open with line numbers displayed on the left:
$ nano -l app.py

# Open in read-only / view mode (prevents accidental edits):
$ nano -v config.yml
```

---

## 2. Interface & Symbol Notation

At the bottom of the nano screen, a two-row keyboard shortcut legend is displayed:
- The caret symbol **`^`** denotes the **`Ctrl`** key (e.g., `^O` means `Ctrl + O`).
- The **`M-`** prefix denotes the **`Alt`** (Meta) key (e.g., `M-U` means `Alt + U`).

```
┌─────────────────────────────────────────────────────────────┐
│  GNU nano 6.2                  app.py                       │
├─────────────────────────────────────────────────────────────┤
│  1  import os                                               │
│  2  print("Hello Linux")                                    │
├─────────────────────────────────────────────────────────────┤
│^G Help      ^O WriteOut   ^W Where Is   ^K Cut        ^J Justify │
│^X Exit      ^R Read File  ^\ Replace    ^U Paste      ^C Cur Pos │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Essential Nano Keyboard Shortcuts

| Shortcut | Function | Description |
|---|---|---|
| **`Ctrl + O`** | **WriteOut (Save)** | Saves changes to disk (press `Enter` to confirm filename). |
| **`Ctrl + X`** | **Exit** | Closes nano. If unsaved edits exist, prompts `Y` or `N`. |
| **`Ctrl + W`** | **Where Is (Search)** | Opens text search prompt; press `Enter` to find match. |
| **`Alt + W`** | **Search Next** | Repeats the last search forward. |
| **`Ctrl + \`** | **Replace** | Interactive search and replace prompt. |
| **`Ctrl + K`** | **Cut Line** | Cuts the current line into the clipboard buffer. |
| **`Ctrl + U`** | **Uncut (Paste)** | Pastes the cut line buffer at the current cursor position. |
| **`Ctrl + _`** or **`Alt + G`**| **Go To Line** | Prompts for line and column number to jump directly. |
| **`Ctrl + C`** | **Cursor Position** | Displays current line number, column, and total characters. |
| **`Ctrl + G`** | **Help** | Displays the full reference manual. |

---

## 4. Configuring Nano (`~/.nanorc`)

To give nano modern editor behavior (syntax highlighting, 4-space soft tabs, line numbers), create `~/.nanorc`:

```ini
# Display line numbers in the left margin:
set linenumbers

# Convert Tab key into 4 spaces (soft tabs for YAML / Python):
set tabsize 4
set tabstospaces

# Enable smooth scrolling (scroll line by line instead of half-page jumps):
set smooth

# Enable mouse support (click to move cursor):
set mouse
```
