# 05 - Publishing Modules to Terraform Registry

## 1. Registry Requirements

- Repository must be named `terraform-<PROVIDER>-<NAME>`
- Must use semantic versioning (Git tags: `v1.0.0`, `v1.1.0`)
- Must have `README.md`, `main.tf`, `variables.tf`, `outputs.tf`
- Repository must be public on GitHub

## 2. Module Structure for Registry

```text
terraform-aws-vpc/
├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf
├── README.md
├── CHANGELOG.md
├── examples/
│   ├── simple/
│   │   └── main.tf
│   └── complete/
│       └── main.tf
└── modules/           # Sub-modules (optional)
    └── subnets/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

## 3. Publishing Workflow

```bash
# Tag a release
git tag -a v1.0.0 -m "Initial release"
git push origin v1.0.0

# Registry auto-detects new tags and publishes the module
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Module Design Patterns and Best Practices](./04-Module-Design-Patterns-and-Best-Practices.md) | [Index](../../../README.md) | [06 - Module Testing with Terratest and terraform test →](./06-Module-Testing-with-Terratest-and-terraform-test.md) |
