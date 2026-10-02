# 06 - Designing Enterprise Branching Strategies

## 1. Workflow Selection Matrix

| Organization / Product Type | Recommended Workflow | Rationale |
|---|---|---|
| **SaaS / Web Microservices** | **Trunk-Based Development** | Continuous delivery, high deploy frequency, automated CI/CD |
| **Mobile Apps (iOS/Android)** | **Release-Branching TBD** | App Store review delays require stabilizing release branches |
| **Open Source Libraries / SDKs** | **GitHub Flow + Tags** | Public contribution model with semantic tagged releases |
| **Enterprise Regulated On-Prem Software** | **GitLab Flow (Release Branches)** | Must maintain long-term support (LTS) for multiple versions (e.g. v1.x, v2.x) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - SemVer & Git Tags](./05-Semantic-Versioning-and-Git-Tags.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
