# 09 - Git Workflows: Interview Questions & Answers

### Q1: Why do modern high-performing DevOps teams favor Trunk-Based Development over GitFlow?
**Answer:** GitFlow promotes long-lived branches and batch releases, leading to painful merge conflicts, delayed feedback loops, and broken CI/CD pipelines. Trunk-Based Development encourages small, frequent commits directly into the trunk (< 24h branches) with automated CI testing and feature flags, enabling high deployment frequency and rapid mean time to recovery (MTTR).

### Q2: What is the difference between a lightweight tag and an annotated tag?
**Answer:** A lightweight tag is simply a pointer to a specific commit. An annotated tag is stored as a full object in Git's database containing the tagger's identity, timestamp, an optional cryptographic GPG signature, and a dedicated release message.

### Q3: How do Feature Flags facilitate Continuous Delivery in Trunk-Based Development?
**Answer:** Feature flags decouple software deployment from business release. Incomplete or experimental code can be safely merged into `main` and deployed to production in a dormant state, eliminating the need for long-lived isolation branches.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
