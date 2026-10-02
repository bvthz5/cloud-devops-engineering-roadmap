# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What is the correct module source format for the Terraform Registry?
- [ ] A) `source = "https://registry.terraform.io/modules/aws/vpc"`
- [x] B) `source = "terraform-aws-modules/vpc/aws"`
- [ ] C) `source = "registry://aws/vpc"`
- [ ] D) `source = "aws::vpc::module"`

<details>
<summary>Explanation</summary>
Registry modules use the format NAMESPACE/NAME/PROVIDER. The registry URL is implicit.
</details>

---

### Question 2
What happens if you don't specify a `version` constraint when using a registry module?
- [ ] A) Terraform refuses to download the module
- [ ] B) Terraform uses version 1.0.0
- [x] C) Terraform downloads the latest version, risking breaking changes
- [ ] D) Terraform uses the version from .terraform.lock.hcl

<details>
<summary>Explanation</summary>
Without a version constraint, Terraform downloads the latest available version. This can introduce breaking changes if a new major version is released.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
