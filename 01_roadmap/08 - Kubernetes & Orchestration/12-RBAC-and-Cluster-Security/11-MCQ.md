# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which command allows an administrator to test whether a ServiceAccount has permission to create pods?
- [ ] A) `kubectl rbac test --user sa`
- [x] B) `kubectl auth can-i create pods --as=system:serviceaccount:default:my-sa`
- [ ] C) `kubectl check access my-sa`
- [ ] D) `kubectl get permissions --sa my-sa`

<details>
<summary>Explanation</summary>
`kubectl auth can-i` evaluates the API server's authorizer rules for arbitrary users, groups, or service accounts using the `--as` flag.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
