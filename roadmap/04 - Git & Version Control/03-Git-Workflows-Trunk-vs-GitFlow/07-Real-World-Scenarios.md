# 07 - Git Workflows: Real-World Production Scenarios

## Scenario 1: The Three-Month Branch Merge Paralysis

### Incident Summary
An enterprise using GitFlow kept a major refactoring branch (`feature/v2-auth`) open for 4 months while 20 other developers continued shipping to `develop`. When the team finally attempted to merge `feature/v2-auth`, Git reported **640 file merge conflicts**. Resolving conflicts took 3 weeks, broke production functionality, and caused team morale collapse.

### Resolution & Transition to TBD
The organization mandated **Trunk-Based Development**:
1. All future refactoring broken into small PRs (< 200 lines) merged daily.
2. Unfinished modules hidden behind feature flags.
3. Merge conflicts dropped by 95%, and deployment frequency increased from monthly to 6 times daily.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Designing Enterprise Branching Strategies](./06-Designing-Enterprise-Branching-Strategies.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
