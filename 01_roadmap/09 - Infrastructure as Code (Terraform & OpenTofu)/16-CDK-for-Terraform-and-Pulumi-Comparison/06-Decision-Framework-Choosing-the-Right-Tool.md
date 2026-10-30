# 06 - Decision Framework: Choosing the Right Tool

```text
Need multi-cloud?
+-- NO --> Use cloud-native (CloudFormation, Bicep)
+-- YES --> Team prefers DSL?
            +-- YES --> Terraform / OpenTofu (HCL)
            +-- NO  --> Team prefers programming language?
                        +-- YES --> Pulumi (native) or CDKTF (Terraform backend)
                        +-- NO  --> Terraform / OpenTofu (HCL)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Migration Between Tools](./05-Migration-Between-Tools.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
