# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which volume binding mode ensures that a cloud disk is provisioned in the exact same Availability Zone where the pod gets scheduled?
- [ ] A) Immediate
- [x] B) WaitForFirstConsumer
- [ ] C) LazyBinding
- [ ] D) ZoneAffinityStrict

<details>
<summary>Explanation</summary>
`volumeBindingMode: WaitForFirstConsumer` delays binding and dynamic provisioning until a Pod using the PVC is assigned to a node, guaranteeing AZ alignment.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
