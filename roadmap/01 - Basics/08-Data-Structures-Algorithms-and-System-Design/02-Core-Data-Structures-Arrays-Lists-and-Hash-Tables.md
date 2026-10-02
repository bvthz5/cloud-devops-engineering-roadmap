# 02 — Core Data Structures: Arrays, Lists, and Hash Tables

The underlying physical memory organization of a data structure determines its CPU cache efficiency and access latency.

---

## 1. Arrays vs Linked Lists (CPU Cache Locality)

- **Array:** Stores elements in a **contiguous block of memory**.
  - Index lookup is instantaneous ($O(1)$) via pointer math: `Address = Base + (Index * ElementSize)`.
  - **Superb CPU Cache Locality:** Hardware prefetchers load adjacent array elements into L1/L2 CPU caches automatically.
- **Linked List:** Stores nodes scattered throughout memory, connected by pointers.
  - Traversal requires following memory pointers ($O(N)$).
  - Poor cache locality (causes frequent CPU cache misses).

---

## 2. Hash Tables / Hash Maps (The Workhorse of Caching)

A Hash Table maps keys to values using a **Hash Function**:
```text
Key ("user_104") ──[ Hash Function ]──> Hash (0x8F3B) ──[ Modulo Array Size ]──> Bucket Index [5]
```

### Collision Resolution:
When two different keys hash to the same bucket index:
1. **Chaining:** Each bucket contains a linked list of entries that share that index.
2. **Open Addressing:** If bucket is occupied, probe the next available adjacent slot.

- **Lookup Time:** Average **$O(1)$**, Worst-case **$O(N)$** (if poor hash function causes all keys to collide into a single bucket).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Algorithmic Complexity](./01-Algorithmic-Complexity-and-Big-O-Notation.md) | [README](./README.md) | [03 - Consistent Hashing](./03-Consistent-Hashing-in-Distributed-Systems.md) |
