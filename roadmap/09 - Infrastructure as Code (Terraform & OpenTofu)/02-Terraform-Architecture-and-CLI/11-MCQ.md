# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
How do Terraform providers communicate with the Terraform Core binary?
- [ ] A) REST API calls over HTTP
- [x] B) gRPC plugin protocol (v5/v6)
- [ ] C) Shared memory IPC
- [ ] D) Unix domain sockets with JSON-RPC

<details>
<summary>Explanation</summary>
Terraform Core and its provider plugins communicate via the gRPC-based plugin protocol. This allows providers to be developed independently as separate binaries.
</details>

---

### Question 2
Which file should ALWAYS be committed to version control?
- [ ] A) `.terraform/` directory
- [ ] B) `terraform.tfstate`
- [x] C) `.terraform.lock.hcl`
- [ ] D) `crash.log`

<details>
<summary>Explanation</summary>
The dependency lock file (.terraform.lock.hcl) ensures all team members use identical provider versions. State files contain sensitive data and should be stored remotely, never committed.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
