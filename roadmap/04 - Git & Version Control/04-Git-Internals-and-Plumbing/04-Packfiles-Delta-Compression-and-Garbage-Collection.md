# 04 - Packfiles, Delta Compression, and Garbage Collection

## 1. Loose Objects vs. Packed Objects

- **Loose Objects:** When you commit a file, Git stores it as an individual zlib-compressed file in `.git/objects/xx/yyy...`. If you edit one line in a 10 MB file, Git creates a second 10 MB loose object.
- **Packfiles (`.pack`):** To optimize disk space and network bandwidth, Git periodically combines hundreds of loose objects into a single compressed binary **Packfile**, paired with an **Index File (`.idx`)** for rapid byte-offset lookups.

```
.git/objects/pack/
├── pack-d1234...idx   # Fast binary search index (SHA -> file byte offset)
└── pack-d1234...pack  # Compressed concatenation of objects with delta compression
```

---

## 2. Delta Compression Mechanics

In a Packfile, Git does **not** store duplicate full files. It uses **Sliding Window Delta Compression**:
- It identifies the most recent version of a file and stores it **in full** (so current checkouts are fast).
- It stores older versions as **reverse deltas** (differences from the newer version).

---

## 3. Garbage Collection (`git gc`)

`git gc` cleans up loose objects, packs references, compresses objects, and deletes stale unreachable objects:

```bash
# Standard garbage collection
git gc

# Aggressive compression (recomputes optimal deltas; CPU-intensive)
git gc --aggressive

# Force immediate pruning of all unreferenced/dangling objects
git gc --prune=now
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Plumbing vs Porcelain](./03-Plumbing-vs-Porcelain-Commands-Deep-Dive.md) | [README](./README.md) | [05 - DAG & Reachability](./05-The-Directed-Acyclic-Graph-DAG-and-Reachability.md) |
