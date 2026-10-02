# 09 - Interview Questions

### Q1: How do Terraform providers communicate with the core binary?
**Answer:** Via gRPC plugin protocol (v5/v6). The core binary spawns the provider as a child process and communicates over gRPC. The provider implements CRUD operations that translate into API calls to the target service.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
