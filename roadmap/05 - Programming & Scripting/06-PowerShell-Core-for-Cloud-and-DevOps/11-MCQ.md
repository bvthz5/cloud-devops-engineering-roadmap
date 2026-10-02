# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which stream in PowerShell is dedicated exclusively to structured informational messages?
- [ ] A) Stream 1
- [ ] B) Stream 3
- [x] C) Stream 6
- [ ] D) Stream 4

<details>
<summary><b>Explanation</b></summary>
Stream 1 is Success/Output, Stream 2 is Error, Stream 3 is Warning, Stream 4 is Verbose, Stream 5 is Debug, and Stream 6 is Information.
</details>

---

### 2. How must a variable in the outer scope be referenced inside a `ForEach-Object -Parallel` script block?
- [ ] A) `$global:myVar`
- [x] B) `$using:myVar`
- [ ] C) `$parent:myVar`
- [ ] D) `$env:myVar`

<details>
<summary><b>Explanation</b></summary>
In multi-threaded runspaces created by <code>ForEach-Object -Parallel</code>, outer scoped variables must be prefixed with the <code>$using:</code> scope modifier to be passed across runspace boundaries.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
