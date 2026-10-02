# 11 - Golang for Cloud-Native: Self-Assessment MCQs

### Q1. How much initial stack memory does a Go goroutine consume?
- A) 1 MB
- B) 512 KB
- C) ~2 KB
- D) 64 KB
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>Goroutines start with an ultra-lightweight ~2 KB stack that expands dynamically.</details>

---

### Q2. Which standard library package carries deadlines and cancellation signals across goroutine trees?
- A) `sync`
- B) `context`
- C) `time`
- D) `os/signal`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`context.Context` is the standard Go mechanism for cancellation and deadlines.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
