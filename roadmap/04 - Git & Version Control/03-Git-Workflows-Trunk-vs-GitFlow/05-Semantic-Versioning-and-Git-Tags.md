# 05 - Semantic Versioning and Git Tags

## 1. Semantic Versioning Specification (SemVer 2.0)

Version format: `MAJOR.MINOR.PATCH` (e.g. `v2.4.1`)

```
   v2 . 4 . 1
    │   │   │
    │   │   └── PATCH: Backward-compatible bug fixes
    │   └────── MINOR: Backward-compatible new functionality
    └────────── MAJOR: Incompatible, breaking API changes
```

---

## 2. Lightweight vs. Annotated Git Tags

- **Lightweight Tag:** Simply a pointer to a specific commit (like an immutable branch).
- **Annotated Tag (Recommended for Releases):** Stored as a full object in the Git database. Contains tagger name, email, timestamp, GPG signature, and a release message.

```bash
# Create annotated release tag
git tag -a v1.2.0 -m "Release v1.2.0: Adds Stripe payment support"

# Push tag to remote
git push origin v1.2.0

# Push all local tags
git push origin --tags

# Verify tag signature and metadata
git show v1.2.0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Feature Flags Decoupling Deploy from Release](./04-Feature-Flags-Decoupling-Deploy-from-Release.md) | [Index](../../../README.md) | [06 - Designing Enterprise Branching Strategies →](./06-Designing-Enterprise-Branching-Strategies.md) |
