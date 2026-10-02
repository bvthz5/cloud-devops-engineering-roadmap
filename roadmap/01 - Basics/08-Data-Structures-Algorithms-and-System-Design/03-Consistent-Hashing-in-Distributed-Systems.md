# 03 — Consistent Hashing in Distributed Systems

In distributed architectures, data is partitioned across multiple cache or database nodes. Standard modulo hashing fails catastrophically when nodes are added or removed.

---

## 1. The Flaw of Traditional Modulo Hashing

Formula: `Node = hash(key) % N` (where $N$ is the number of nodes).
- If you have 4 cache servers ($N=4$) and add 1 new server ($N=5$):
- **Almost all existing keys remap to different nodes (~80% of all keys move)!**
- This causes a **Cache Stampede / Thundering Herd**: all backend databases are simultaneously slammed by millions of cache misses, bringing down production!

---

## 2. The Consistent Hashing Ring

Consistent Hashing maps both **nodes** and **keys** to a 360-degree circle (e.g. 0 to $2^{32}-1$):

```text
                       Node A (at 45°)
                       /             \
      Key 3 (at 350°)                 Key 1 (at 90°)
            |                               |
       [ HASH RING: 0 to 2^32 - 1 ]         v
            |                         Node B (at 180°)
            v                               |
      Node C (at 270°) <────────────── Key 2 (at 220°)
```

1. To find which node owns a key, hash the key to a position on the ring and **walk clockwise** until you hit a node.
2. **When adding a new Node:** Only keys between the new node and its counter-clockwise neighbor are moved. **Only $K/N$ keys are remapped!**
3. **Virtual Nodes:** To prevent hotspots where one node gets more traffic, each physical server is assigned 100–200 "virtual nodes" scattered randomly across the ring.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Core Data Structures Arrays Lists and Hash Tables](./02-Core-Data-Structures-Arrays-Lists-and-Hash-Tables.md) | [Index](../../../README.md) | [04 - Stacks Queues and Message Brokers →](./04-Stacks-Queues-and-Message-Brokers.md) |
