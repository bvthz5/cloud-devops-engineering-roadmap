# 02 - Multi-Team Collaboration Patterns

## 1. Module Ownership

```text
CODEOWNERS file:
  /modules/vpc/         @platform-team
  /modules/eks/         @platform-team
  /services/api/        @api-team
  /services/frontend/   @frontend-team
```

## 2. Producer-Consumer Model

```text
Platform Team (Producers)     Application Teams (Consumers)
  |                               |
  v                               v
Build reusable modules           Use modules with approved inputs
Enforce standards via modules    Can't modify underlying resources
Publish to private registry     Pin module versions
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Monorepo vs Polyrepo for IaC](./01-Monorepo-vs-Polyrepo-for-IaC.md) | [Index](../../../README.md) | [03 - State Architecture at Scale →](./03-State-Architecture-at-Scale.md) |
