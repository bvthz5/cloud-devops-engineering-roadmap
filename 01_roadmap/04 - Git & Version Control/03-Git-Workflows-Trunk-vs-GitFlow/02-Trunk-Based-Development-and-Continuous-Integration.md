# 02 - Trunk-Based Development and Continuous Integration

## 1. What is Trunk-Based Development (TBD)?

In **Trunk-Based Development**, all engineers commit to a single shared branch ("trunk" or `main`) frequently (often multiple times per day).

```
main (Trunk) ──► ● ───────► ● ────────► ● ───────► ● ───────► Continuous Deploy
                  \       ▲  \        ▲
      Short-lived  ● ─────┘   ● ──────┘ (< 24 hours lifespan)
      feature branch
```

### Core Principles:
1. **Short-Lived Branches:** Feature branches last less than 1 day (hours, not weeks).
2. **Small Batches:** Commits represent small incremental updates (< 300 lines of code).
3. **Automated CI Validation:** Fast, automated test suites (under 10 minutes) run on every pull request.
4. **DORA Metrics Core:** DORA research confirms Trunk-Based Development is one of the strongest statistical predictors of high software delivery performance.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - GitFlow Architecture and Lifecycle](./01-GitFlow-Architecture-and-Lifecycle.md) | [Index](../../../README.md) | [03 - GitHub Flow and GitLab Flow Comparison →](./03-GitHub-Flow-and-GitLab-Flow-Comparison.md) |
