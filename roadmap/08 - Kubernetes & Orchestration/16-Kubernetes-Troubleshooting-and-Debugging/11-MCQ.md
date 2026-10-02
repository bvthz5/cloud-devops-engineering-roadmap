# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which command allows an engineer to view the logs of a container's previous crashed instance?
- [ ] A) `kubectl logs <pod> --history`
- [x] B) `kubectl logs <pod> --previous`
- [ ] C) `kubectl logs <pod> --crash`
- [ ] D) `kubectl get logs --last`

<details>
<summary>Explanation</summary>
`--previous` (or `-p`) retrieves the logs for the previous instance of the container in a pod if it has restarted.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
