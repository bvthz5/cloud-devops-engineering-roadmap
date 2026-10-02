# 04 - HCL vs General-Purpose Language Tradeoffs

| Aspect | HCL (Terraform) | GPL (Pulumi/CDKTF) |
|---|---|---|
| Learning curve | Lower (purpose-built) | Higher (must know Python/TS/Go) |
| IDE support | Good (HCL plugins) | Excellent (full language IntelliSense) |
| Testing | terraform test (limited) | Standard test frameworks (pytest, jest) |
| Abstraction | Modules | Classes, functions, inheritance |
| Conditionals | Ternary + count/for_each | Full if/else, loops, error handling |
| Community | Largest IaC community | Growing |
| Ecosystem | 3,500+ providers | Same providers (CDKTF) / own SDKs (Pulumi) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Pulumi Architecture](./03-Pulumi-Architecture-and-State.md) | [README](./README.md) | [05 - Migration](./05-Migration-Between-Tools.md) |
