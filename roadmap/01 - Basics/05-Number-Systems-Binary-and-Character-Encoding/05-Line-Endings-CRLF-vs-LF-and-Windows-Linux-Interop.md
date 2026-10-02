# 05 — Line Endings: CRLF vs LF and Windows-Linux Interop

The mismatch between Windows and Unix line breaks is one of the most common causes of CI/CD build failures and container startup crashes.

---

## 1. The Historical Origin

In mechanical typewriters:
- **Carriage Return (CR - `` - ASCII 13 / `0x0D`):** Moved the printing carriage back to the left margin.
- **Line Feed (LF - `
` - ASCII 10 / `0x0A`):** Rolled the paper up by one line.

| Operating System | Line Ending Characters | Hexadecimal | Escape Sequence |
| :--- | :--- | :--- | :--- |
| **Unix / Linux / macOS**| Line Feed only | `0x0A` | `
` (`LF`) |
| **Windows / DOS** | Carriage Return + Line Feed | `0x0D 0x0A` | `
` (`CRLF`) |

---

## 2. The Classic Docker Disaster: `^M: bad interpreter`

When a shell script (e.g. `entrypoint.sh`) is written or checked out on Windows with CRLF line endings and mounted into a Linux container:
```text
#!/bin/bash

```
The Linux kernel reads the shebang line up to `
`. It looks for an executable named:
`/bin/bash` (with a trailing invisible Carriage Return character!).
Because no file named `/bin/bash` exists, the container crashes with:
```text
/bin/bash^M: bad interpreter: No such file or directory
# Or:
entrypoint.sh: line 2: $'': command not found
```

---

## 3. How to Diagnose and Fix Line Endings

```bash
# 1. Detect carriage returns using 'file' command
file entrypoint.sh
# Output: entrypoint.sh: Bourne-Again shell script, ASCII text executable, with CRLF line terminators

# 2. View hidden  characters using 'cat -v'
cat -v entrypoint.sh
# Output: #!/bin/bash^M

# 3. Convert CRLF to LF using dos2unix
sudo apt-get install -y dos2unix
dos2unix entrypoint.sh

# 4. Or convert using sed / tr (no extra tools needed)
sed -i 's/$//' entrypoint.sh
```

### Git Repository Protection (`.gitattributes`):
Add to your project root to force Git to always check out Linux line endings:
```text
* text=auto eol=lf
*.sh text eol=lf
Dockerfile text eol=lf
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Character Encoding](./04-Character-Encoding-ASCII-Unicode-and-UTF8.md) | [README](./README.md) | [06 - Data Encoding Standards](./06-Data-Encoding-Standards-Base64-Hex-URL-Encoding.md) |
