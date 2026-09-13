# 06 - Search and Replace

VI provides both real-time interactive searching and powerful regex-based global substitution across the entire file or selected regions.

---

## 🔍 Searching (Normal Mode)

| Command | Action |
| :---: | :--- |
| `/pattern` | Search **forward** for `pattern` from cursor position |
| `?pattern` | Search **backward** for `pattern` from cursor position |
| `n` | Jump to the **next** match (same direction as original search) |
| `N` | Jump to the **previous** match (opposite direction) |
| `*` | Search forward for the word **currently under the cursor** |
| `#` | Search backward for the word **currently under the cursor** |

### Disabling Search Highlight After Searching
```text
:noh    (No Highlight - clear the current search highlight)
```

---

## 🔄 Search & Replace (Substitution — Command-Line Mode)

Vim's substitution uses the `:s` (substitute) command with the standard Ex format:

```text
:[range]s/search_pattern/replacement/[flags]
```

### Substitution Range Selectors

| Range Syntax | Scope |
| :--- | :--- |
| `:s/old/new/` | Replace **first** occurrence on **current line only** |
| `:s/old/new/g` | Replace **all** occurrences on **current line** |
| `:%s/old/new/g` | Replace **all** occurrences in the **entire file** (`%` = all lines) |
| `:5,20s/old/new/g` | Replace all occurrences between **lines 5 and 20** |
| `:'<,'>s/old/new/g` | Replace within **Visual Mode selection** |

### Essential Substitution Flags

| Flag | Meaning |
| :---: | :--- |
| `g` | **Global** — replace all matches on line (not just first) |
| `c` | **Confirm** — prompts interactively before each replacement (`y/n/a/q/l`) |
| `i` | **Case-insensitive** matching |
| `I` | **Case-sensitive** matching (overrides `ignorecase` setting) |

### Practical DevOps Examples

1. **Replace `localhost` with `10.0.1.50` across the entire nginx config file:**
   ```text
   :%s/localhost/10.0.1.50/g
   ```

2. **Replace `staging` with `production` everywhere, with interactive confirmation:**
   ```text
   :%s/staging/production/gc
   ```

3. **Delete all blank/empty lines in a file:**
   ```text
   :g/^$/d
   ```

4. **Add `#` comment prefix to lines 5 through 15 (commenting out a block):**
   ```text
   :5,15s/^/# /
   ```

5. **Remove leading whitespace from all lines:**
   ```text
   :%s/^\s\+//
   ```
