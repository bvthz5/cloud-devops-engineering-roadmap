# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which ECR repository setting guarantees that once an image tag is pushed, it can never be overwritten?
- [ ] A) `ReadOnly: true`
- [x] B) `imageTagMutability: IMMUTABLE`
- [ ] C) `enableProtection: true`
- [ ] D) `lockTags: true`

<details>
<summary><b>Explanation</b></summary>
Setting <code>imageTagMutability: IMMUTABLE</code> prevents any subsequent push from overwriting an existing tag.
</details>

---

### 2. How long is an AWS ECR authorization token valid for after running `aws ecr get-login-password`?
- [ ] A) 1 hour
- [x] B) 12 hours
- [ ] C) 24 hours
- [ ] D) Indefinitely

<details>
<summary><b>Explanation</b></summary>
AWS ECR authorization tokens generated via STS expire after exactly 12 hours.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
