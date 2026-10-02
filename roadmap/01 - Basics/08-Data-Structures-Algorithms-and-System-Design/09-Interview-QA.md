# 09 — Data Structures & Algorithms Interview Q&A

10 technical interview questions for DevOps, SRE, and Systems Engineering roles.

---

### Q1: Why do relational databases use B-Trees instead of Hash Indexes for primary indexes?
**Answer:**
A Hash Index provides $O(1)$ point lookups (`WHERE id = 5`), but **cannot perform range queries** (`WHERE age BETWEEN 20 AND 30`) or sorted order queries (`ORDER BY date`) because hashes distribute data randomly.
A B+ Tree keeps data sorted. It provides $O(\log N)$ lookups while effortlessly supporting range scans and sorting by traversing leaf node pointers.

---

### Q2: What problem does Consistent Hashing solve in distributed caching?
**Answer:**
Traditional modulo hashing (`hash(key) % N`) causes almost all keys (~$(N-1)/N$ of the dataset) to remap to different servers when a cache server is added or removed, resulting in catastrophic cache stampedes.
Consistent Hashing maps both keys and servers to a circular ring, ensuring that when a server scales up or down, only $1/N$ fraction of the keys need to be relocated.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
