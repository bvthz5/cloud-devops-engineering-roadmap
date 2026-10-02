# 01 - GitFlow Architecture and Lifecycle

## 1. What is GitFlow?

Created by Vincent Driessen in 2010, **GitFlow** is a strict, branch-heavy workflow designed around scheduled, batch releases.

```
master ────────────────────────────────────────► (v1.0) ───────────────────► (v1.1)
  ▲                                                ▲                           ▲
  │                                                │                           │
hotfix ────────────────────────────────────► [hotfix-1.0.1]                    │
  │                                                ▲                           │
develop ──► [feat-A] ──► [feat-B] ──► [release-1.1] ───────────────────────────┘
```

### The 5 Core Branches:
1. `master` / `main`: Production-ready code only. Tagged with release versions.
2. `develop`: Integration branch for nightly builds and features.
3. `feature/*`: Branch off `develop`, merge back into `develop`.
4. `release/*`: Branch off `develop` when preparing a milestone release. Only bug fixes allowed. Merged into BOTH `master` and `develop`.
5. `hotfix/*`: Branch off `master` to patch emergency production bugs. Merged into BOTH `master` and `develop`.

---

## 2. Why Modern DevOps Replaced GitFlow
In continuous delivery environments deploying 20+ times per day:
- **Merge Hell:** Merging `release` and `hotfix` branches back into both `master` and `develop` produces cascading merge conflicts.
- **Batching Antipattern:** Features sit unreleased on `develop` for weeks, violating small-batch CI/CD principles.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Trunk-Based Development](./02-Trunk-Based-Development-and-Continuous-Integration.md) |
