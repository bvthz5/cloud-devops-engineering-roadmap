# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
In a Deployment with `replicas: 4`, `maxSurge: 1`, and `maxUnavailable: 0`, what is the maximum number of Pods that can be running simultaneously during an update?
- [ ] A) 4
- [x] B) 5
- [ ] C) 6
- [ ] D) 3

<details>
<summary>Explanation</summary>
`maxSurge: 1` allows the total number of pods to exceed the desired replica count by 1 during the rollout ($4 + 1 = 5$).
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
