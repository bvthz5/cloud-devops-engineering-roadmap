# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which of the following is the PRIMARY advantage of declarative IaC over imperative scripting?
- [ ] A) Faster execution speed
- [x] B) Built-in idempotency and drift detection
- [ ] C) More programming language choices
- [ ] D) Lower learning curve

<details>
<summary>Explanation</summary>
Declarative IaC tools automatically compare desired state with actual state and only make necessary changes. This idempotency means applying the same configuration multiple times produces no unintended side effects.
</details>

---

### Question 2
What event triggered the creation of OpenTofu?
- [ ] A) Terraform reached end-of-life
- [x] B) HashiCorp relicensed Terraform from MPL 2.0 to BSL 1.1
- [ ] C) AWS acquired HashiCorp
- [ ] D) Terraform stopped supporting multi-cloud providers

<details>
<summary>Explanation</summary>
In August 2023, HashiCorp changed Terraform's license from MPL 2.0 (fully open source) to BSL 1.1 (restricts competitive use). This prompted 140+ organizations to fork Terraform into OpenTofu under the Linux Foundation.
</details>

---

### Question 3
A "snowflake server" anti-pattern means:
- [ ] A) A server running in a cold climate region
- [ ] B) A server with automated configuration management
- [x] C) A manually configured server whose exact state is unknown and unreproducible
- [ ] D) A server using immutable container images

<details>
<summary>Explanation</summary>
Snowflake servers accumulate manual changes over time until no one can reproduce their configuration. IaC and immutable infrastructure patterns eliminate this problem.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
