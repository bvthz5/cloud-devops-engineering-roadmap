# 01 — The Build Pipeline: Source to Machine Code

When you run `gcc main.c -o app` or `go build`, a series of specialized compiler tools process your source code through four distinct stages.

---

## 1. The 4 Compilation Stages

```text
Source Code (.c / .cpp)
       │
       ▼  [ 1. Preprocessor: cpp ]
  - Expands #include headers
  - Replaces #define macros
  - Strips comments
       │
       ▼
Preprocessed Source (.i)
       │
       ▼  [ 2. Compiler: cc1 ]
  - Lexical analysis (Tokenization)
  - Syntax & Semantic parsing (AST - Abstract Syntax Tree)
  - Optimization (-O2, -O3)
  - Generates target CPU assembly code
       │
       ▼
Assembly Code (.s)
       │
       ▼  [ 3. Assembler: as ]
  - Translates mnemonic assembly instructions to machine opcodes
  - Generates unlinked machine code Object File
       │
       ▼
Object File (.o / .obj)
       │
       ▼  [ 4. Linker: ld ]
  - Resolves external function references (e.g. printf from libc)
  - Merges data and code sections
  - Emits final Executable and Linkable Format (ELF) binary
       │
       ▼
Executable Binary (ELF on Linux, PE on Windows, Mach-O on macOS)
```

---

## 2. Linux Executable Format: ELF

On Linux, executables, shared libraries (`.so`), and core dumps are formatted as **ELF (Executable and Linkable Format)**.
Key sections in an ELF binary:
- `.text`: The actual executable machine code instructions.
- `.rodata`: Read-only constants (string literals).
- `.data`: Initialized global and static variables.
- `.bss`: Uninitialized global and static variables (zero-initialized at runtime).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Number-Systems-Binary-and-Character-Encoding)](../05-Number-Systems-Binary-and-Character-Encoding/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Static vs Dynamic Linking and Shared Libraries →](./02-Static-vs-Dynamic-Linking-and-Shared-Libraries.md) |
