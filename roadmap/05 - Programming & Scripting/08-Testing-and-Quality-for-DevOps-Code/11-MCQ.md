# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which ShellCheck warning code flags unquoted variable expansion that could cause globbing and word splitting?
- [ ] A) SC1000
- [x] B) SC2086
- [ ] C) SC2154
- [ ] D) SC2181

<details>
<summary><b>Explanation</b></summary>
ShellCheck SC2086 specifically flags missing double quotes around variables (e.g. <code>$foo</code> instead of <code>"$foo"</code>), which leads to word splitting and path globbing hazards.
</details>

---

### 2. In Go unit testing, which flag compiles tests with thread race condition instrumentation?
- [ ] A) `-bench`
- [ ] B) `-cover`
- [x] C) `-race`
- [ ] D) `-atomic`

<details>
<summary><b>Explanation</b></summary>
The <code>-race</code> flag invokes Go's built-in race detector, instrumenting memory accesses to detect data races at runtime.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
