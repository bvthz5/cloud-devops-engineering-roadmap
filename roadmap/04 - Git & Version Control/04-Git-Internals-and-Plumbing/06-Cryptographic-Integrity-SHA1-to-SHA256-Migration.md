# 06 - Cryptographic Integrity: SHA-1 to SHA-256 Migration

## 1. Cryptographic Hash Roles in Git

Git uses cryptographic hashing for:
1. **Object Naming:** The hash is the unique identifier of the object.
2. **Tamper Evidence:** If an attacker modifies even a single byte or timestamp in an old commit, that commit's hash changes. Consequently, all descendant child commit hashes break!

---

## 2. The SHAttered Attack & Git's Defense

In 2017, Google researchers published the **SHAttered attack**, generating two distinct PDF files with the exact same SHA-1 hash.

### Git's Defense: `sha1dc`
Git immediately adopted **SHA-1 Collision Detection (sha1dc)**, which analyzes input for cryptographic attack patterns and safely aborts if collision attempts are detected.

---

## 3. Transition to SHA-256 (Object Format v2)
Git now natively supports SHA-256 repositories:
```bash
# Initialize a modern repository using 64-character SHA-256 hashes
git init --object-format=sha256 my-secure-repo
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - DAG & Reachability](./05-The-Directed-Acyclic-Graph-DAG-and-Reachability.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
