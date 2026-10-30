# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which container exit code indicates an executable was not found inside `$PATH`?
- [ ] A) 1
- [ ] B) 126
- [x] C) 127
- [ ] D) 137

<details>
<summary><b>Explanation</b></summary>
Exit Code 127 indicates "command not found" (the binary specified in ENTRYPOINT/CMD does not exist or is misspelled).
</details>

---

### 2. Which logging option prevents a stalled logging daemon from locking up container standard I/O?
- [ ] A) `mode: async`
- [x] B) `mode: non-blocking`
- [ ] C) `buffer: direct`
- [ ] D) `driver: skip`

<details>
<summary><b>Explanation</b></summary>
Setting <code>mode: non-blocking</code> instructs Docker to buffer logs in memory and drop older entries if the logging consumer lags, protecting application threads.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
