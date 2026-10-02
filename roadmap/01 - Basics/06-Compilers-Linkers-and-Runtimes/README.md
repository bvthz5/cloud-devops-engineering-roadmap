# Submodule 06: Compilers, Linkers, and Runtimes (How Software Runs)

Understanding how source code transforms into running machine instructions in memory is essential for diagnosing container startup errors, building minimal Docker images, profiling CPU/memory bottlenecks, and resolving missing dynamic library failures.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Dissect the 4 phases of compilation: **Preprocessing, Compilation, Assembly, and Linking**.
- Master **Static vs Dynamic Linking** and inspect shared library dependencies using **`ldd`**.
- Understand the critical architectural differences between **glibc and musl libc** (The Alpine Linux Docker trap).
- Analyze the internal **Process Memory Layout** (Text, Data, BSS, Heap, and Stack).
- Compare execution models: **Ahead-of-Time (AOT) Compiled, Interpreted, and Just-In-Time (JIT)**.
- Debug missing dynamic shared object errors (`error while loading shared libraries`).
- Build zero-dependency, ultra-lightweight container binaries (`CGO_ENABLED=0`).

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [The Build Pipeline: Source to Machine Code](./01-The-Build-Pipeline-Source-to-Machine-Code.md) | Preprocessor (`cpp`), Compiler (`gcc`), Assembler (`as`), Linker (`ld`), ELF | ✅ Complete |
| 02 | [Static vs Dynamic Linking & Shared Objects](./02-Static-vs-Dynamic-Linking-and-Shared-Libraries.md) | Archive libraries (`.a`) vs Shared objects (`.so`), `ldd`, and `RPATH` | ✅ Complete |
| 03 | [C Standard Libraries: glibc vs musl libc](./03-C-Standard-Libraries-glibc-vs-musl.md) | The Alpine Linux Docker issue, ABI compatibility, and DNS differences | ✅ Complete |
| 04 | [Process Memory Layout: Heap, Stack & Segments](./04-Process-Memory-Layout-Heap-Stack-and-Segments.md) | Text, Data, BSS, Heap dynamic allocation, Stack frames, memory leaks | ✅ Complete |
| 05 | [Execution Models: Compiled, Interpreted & JIT](./05-Execution-Models-Compiled-Interpreted-and-JIT.md) | Native (Go/Rust/C) vs Interpreted (Python) vs JIT (Java/Node.js) | ✅ Complete |
| 06 | [Dynamic Linker & Symbol Resolution](./06-Dynamic-Linker-and-Symbol-Resolution.md) | `ld-linux.so`, ELF symbol tables, `LD_PRELOAD`, and missing symbol errors | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Container startup failure `sh: ./app: not found`, CGO build breaks | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Debugging with `ldd`, `readelf`, `nm`, `objdump`, and `strace` | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical questions on linkers, runtimes, and memory execution | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Static vs dynamic builds, inspect ELF binaries, compile pure Go for scratch | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Binary inspection tool matrix, memory segment diagrams, and environment vars | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Number Systems](../05-Number-Systems-Binary-and-Character-Encoding/README.md) | [01 - Basics Index](../README.md) | [01 - The Build Pipeline](./01-The-Build-Pipeline-Source-to-Machine-Code.md) |
