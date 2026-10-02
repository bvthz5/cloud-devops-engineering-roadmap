# 04 — Character Encoding: ASCII, Unicode, and UTF-8

Text in computers is an illusion: every character is represented by a numerical code point stored in binary bytes. Encoding mismatches cause garbled text (**Mojibake**) and fatal application crashes.

---

## 1. Evolution of Character Sets

```text
ASCII (1963)                Extended ASCII (1980s)          Unicode (1991 - Present)
7 bits (0-127)              8 bits (0-255)                  Universal Character Set
English alphabet,           Added accented characters       Every character in every human
digits, control chars       (ISO-8859-1, Windows-1252)      language + emojis (>149,000 chars)
```

---

## 2. UTF-8: The Undisputed King of the Internet

UTF-8 (Unicode Transformation Format - 8-bit) is a **variable-length encoding** designed by Ken Thompson and Rob Pike (the creators of Unix and Go):
- Characters 0–127 use **exactly 1 byte** (100% backward compatible with ASCII!).
- Latin, Greek, Cyrillic, Arabic use **2 bytes**.
- Asian characters (Chinese, Japanese, Korean) use **3 bytes**.
- Rare scripts and Emojis (e.g. 🚀 `U+1F680`) use **4 bytes**.

### UTF-8 Binary Bit Pattern:
```text
Byte 1    | Byte 2   | Byte 3   | Byte 4   | Code Point Range
----------+----------+----------+----------+-----------------------------
0xxxxxxx  |          |          |          | U+0000   to U+007F (ASCII)
110xxxxx  | 10xxxxxx |          |          | U+0080   to U+07FF
1110xxxx  | 10xxxxxx | 10xxxxxx |          | U+0800   to U+FFFF
11110xxx  | 10xxxxxx | 10xxxxxx | 10xxxxxx | U+10000  to U+10FFFF (Emojis)
```

---

## 3. The Byte Order Mark (BOM) Hazard

Windows text editors (Notepad) often prepend a 3-byte invisible header (`0xEF, 0xBB, 0xBF`) known as the **UTF-8 BOM** to text files.
- On Linux, the BOM is treated as literal syntax!
- In a bash script: `ï»¿#!/bin/bash` breaks the kernel shebang parser, throwing:
  `bash: ./script.sh: cannot execute binary file: Exec format error`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Endianness & Byte Order](./03-Endianness-Byte-Order-and-Memory-Alignment.md) | [README](./README.md) | [05 - Line Endings CRLF vs LF](./05-Line-Endings-CRLF-vs-LF-and-Windows-Linux-Interop.md) |
