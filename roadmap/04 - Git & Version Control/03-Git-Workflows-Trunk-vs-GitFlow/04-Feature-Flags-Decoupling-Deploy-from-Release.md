# 04 - Feature Flags: Decoupling Deploy from Release

## 1. Deployment vs. Release

- **Deployment (Technical):** Installing code binaries onto production servers.
- **Release (Business):** Making that functionality visible and active for users.

---

## 2. How Feature Flags Enable Trunk-Based Development

In long-running feature projects, engineers historically kept code isolated on branches for months to prevent breaking production.
**With Feature Flags:**
- Incomplete code is merged directly into `main` daily behind a feature toggle:
```python
if feature_flags.is_enabled("NEW_PAYMENT_GATEWAY", user_id):
    process_stripe_v2(order)
else:
    process_legacy_paypal(order)
```
- Code runs safely in production in a dormant state (**Dark Launching**).
- Enable feature for 1% of users (Canary), test performance, and roll out to 100% via dashboard with zero redeployment.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - GitHub & GitLab Flow](./03-GitHub-Flow-and-GitLab-Flow-Comparison.md) | [README](./README.md) | [05 - SemVer & Git Tags](./05-Semantic-Versioning-and-Git-Tags.md) |
