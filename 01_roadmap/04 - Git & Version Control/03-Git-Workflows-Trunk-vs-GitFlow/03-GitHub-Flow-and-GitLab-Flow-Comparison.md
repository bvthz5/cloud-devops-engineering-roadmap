# 03 - GitHub Flow and GitLab Flow Comparison

## 1. GitHub Flow: Simplicity First

GitHub Flow is a streamlined, lightweight workflow ideal for web applications and microservices:

1. Create a descriptive feature branch from `main`.
2. Push commits and open a Pull Request for discussion and review.
3. Once automated tests pass and PR is approved, deploy to staging/production.
4. Merge into `main` immediately upon successful verification.

---

## 2. GitLab Flow: Environment & Release Branches

GitLab Flow adds structured branches while avoiding GitFlow complexity:

### Environment Branches:
```
feature ──► Pull Request ──► main (Trunk) ──► pre-production ──► production
```
- Code flows downstream: `main` automatically deploys to Staging; merging `main` into `production` triggers a production deployment.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Trunk Based Development and Continuous Integration](./02-Trunk-Based-Development-and-Continuous-Integration.md) | [Index](../../../README.md) | [04 - Feature Flags Decoupling Deploy from Release →](./04-Feature-Flags-Decoupling-Deploy-from-Release.md) |
