# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which component in the Kubernetes Control Plane is the **only** one that communicates directly with `etcd`?
- [ ] A) kube-scheduler
- [ ] B) kube-controller-manager
- [x] C) kube-apiserver
- [ ] D) kubelet

<details>
<summary>Explanation</summary>
The `kube-apiserver` acts as the exclusive gatekeeper to `etcd`. All other control plane and worker components must query or modify cluster state via the API server's secure REST endpoints.
</details>

---

### Question 2
In a 5-node etcd cluster, what is the minimum quorum required to commit write operations?
- [ ] A) 2
- [x] B) 3
- [ ] C) 4
- [ ] D) 5

<details>
<summary>Explanation</summary>
Quorum formula is $\lfloor N/2 floor + 1$. For $N = 5$, $\lfloor 5/2 floor + 1 = 2 + 1 = 3$. The cluster can survive the loss of 2 members.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
