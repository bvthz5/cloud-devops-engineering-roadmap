# 04 - sed — Stream Editor

**sed** reads input line by line, applies editing commands to each line, and writes the result to standard output. It is the right tool when you need to **change, transform, or restructure** text — especially without opening a file interactively.

---

## 🔤 Basic Syntax

```bash
sed 'command' file
sed [options] 'command' file
```

---

## 🔄 The Substitute Command — Most Common Use

### First Occurrence Per Line

```bash
sed 's/Engineering/Tech/' employees.txt
# Replaces only the FIRST "Engineering" per line
```

### All Occurrences (Global Flag `g`)

```bash
sed 's/Engineering/Tech/g' employees.txt
# Replaces ALL occurrences per line
```

### Case-Insensitive Substitution

```bash
sed 's/engineering/Tech/gi' employees.txt
# i = case-insensitive, g = global
```

---

## 🧩 Anatomy of the Substitute Command

```text
sed 's/old/new/g'
      │  │   │  │
      │  │   │  └──▶ g = global (replace all on each line)
      │  │   └─────▶ Replacement text
      │  └─────────▶ Pattern to find (supports regex)
      └────────────▶ s = substitute command
```

---

## 💾 In-Place Editing (`-i`)

### Edit Directly in the File

```bash
sed -i 's/Engineering/Tech/g' employees.txt
# ⚠️  This MODIFIES the file permanently. Always test without -i first!
```

### Test First (No -i) — Recommended Workflow

```bash
# Step 1: See the output
sed 's/Engineering/Tech/g' employees.txt

# Step 2: Only if output looks correct, apply in-place
sed -i 's/Engineering/Tech/g' employees.txt
```

### In-Place with a Backup

```bash
sed -i.bak 's/Engineering/Tech/g' employees.txt
# Creates employees.txt.bak before modifying employees.txt
```

---

## 🔀 Alternative Delimiters

The `/` character is the default delimiter. When patterns contain `/` (like file paths), use a different delimiter to avoid escaping:

```bash
# Without alternative delimiter — ugly and error-prone
sed 's/\/home\/alice/\/home\/bob/g' config.txt

# With | as delimiter — clean and readable
sed 's|/home/alice|/home/bob|g' config.txt
```

---

## 📋 Print Selected Lines (`-n` + `p`)

`-n` suppresses all normal output. Combined with `p`, it prints only specified lines:

```bash
sed -n '3p' employees.txt          # Print line 3 only
sed -n '2,5p' employees.txt        # Print lines 2 through 5
sed -n '/Engineering/p' employees.txt  # Print lines matching "Engineering"
```

---

## 🗑️ Delete Lines

```bash
sed '1d' employees.txt             # Delete line 1 (the header)
sed '2,4d' employees.txt           # Delete lines 2 to 4
sed '/Marketing/d' employees.txt   # Delete all Marketing lines
sed '/^$/d' file.txt               # Delete all blank lines
sed '/^#/d' config.conf            # Delete comment lines starting with #
```

---

## ➕ Insert and Append

```bash
# Insert text BEFORE line 2
sed '2i\New line inserted before line 2' employees.txt

# Append text AFTER line 2
sed '2a\New line appended after line 2' employees.txt
```

---

## 🎯 Line-Specific and Pattern-Specific Substitution

```bash
# Substitute ONLY on line 3
sed '3 s/Bob/Robert/' employees.txt

# Substitute ONLY on lines matching "Engineering"
sed '/Engineering/ s/0/X/g' employees.txt
```

---

## 🔁 Multiple Commands (`-e`)

```bash
# Chain multiple substitutions in one command
sed -e 's/Engineering/Tech/g' -e 's/Marketing/Sales/g' employees.txt
```

Alternatively, use a semicolon:

```bash
sed 's/Engineering/Tech/g; s/Marketing/Sales/g' employees.txt
```

---

## 🏭 DevOps Use Cases Summary

```text
┌────────────────────────────────────────────────────────────┐
│  Task                              │  Command Example       │
├────────────────────────────────────┼────────────────────────┤
│  Bulk find/replace in a file       │  sed -i 's/a/b/g' f    │
│  Strip comment lines               │  sed '/^#/d' file      │
│  Strip blank lines                 │  sed '/^$/d' file      │
│  Replace API endpoint in config    │  sed 's|old|new|g' f   │
│  Transform text streams in pipes   │  cmd | sed 's/a/b/g'   │
│  Prepare data for awk processing   │  sed '1d' file | awk   │
│  Preview file without header       │  sed -n '2,$p' file    │
└────────────────────────────────────┴────────────────────────┘
```

---

## ⚠️ Safety Rule

> **Never run `sed -i` on a production file without testing the transformation first.**
>
> Always run without `-i` to verify the output, then apply with a `.bak` backup as a safeguard.
