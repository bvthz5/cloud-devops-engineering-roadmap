# 01 — Algorithmic Complexity and Big-O Notation

Big-O notation describes how execution time or memory requirements of an algorithm scale as the input size ($N$) increases towards infinity.

---

## 1. Common Big-O Time Complexities

```text
Operations
    │                                                   O(2^N) / O(N!) [Horrible]
    │                                              ▲
    │                                          ▲   │     O(N^2) [Bad]
    │                                      ▲   │   │
    │                                  ▲   │   │   │     O(N log N) [Acceptable]
    │                              ▲   │   │   │   │
    │                          ▲   │   │   │   │   │     O(N) [Fair]
    │                      ▲   │   │   │   │   │   │
    │                  ▲   │   │   │   │   │   │   │     O(log N) [Good]
    │  ────────────────────┼───┼───┼───┼───┼───┼───┼───► O(1) [Excellent]
    └───────────────────────────────────────────────────
                               Elements (N)
```

| Notation | Name | Practical Systems Example | Scaling Speed ($N = 10,000$) |
| :--- | :--- | :--- | :---: |
| **$O(1)$** | Constant | Hash map lookup, accessing array index, Redis `GET` | 1 operation |
| **$O(\log N)$** | Logarithmic | Binary search, B-Tree database index lookup | ~13 operations |
| **$O(N)$** | Linear | Linear scan across unindexed database table, reading file | 10,000 operations |
| **$O(N \log N)$**| Linearithmic | Quicksort, Mergesort, compiling dependency graph | ~130,000 operations |
| **$O(N^2)$** | Quadratic | Nested loops (`for i in N: for j in N:`) | 100,000,000 operations |

---

## 2. Why Big-O Matters for DevOps & SREs

In small development environments ($N = 10$ servers or 50 users), an $O(N^2)$ nested loop finishes in 2 milliseconds.
In production with $N = 10,000$ servers or 1,000,000 users, that same script executes $100,000,000$ operations, consuming 100% CPU, locking database rows, and triggering cascading production timeouts!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-Cryptography-PKI-and-Security-Foundations)](../07-Cryptography-PKI-and-Security-Foundations/14-Quick-Revision.md) | [Index](../../../README.md) | [02 - Core Data Structures Arrays Lists and Hash Tables →](./02-Core-Data-Structures-Arrays-Lists-and-Hash-Tables.md) |
