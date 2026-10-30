# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which format should be used for `ENTRYPOINT` to ensure the application process becomes PID 1?
- [ ] A) Shell Form (`ENTRYPOINT node app.js`)
- [x] B) Exec Form (`ENTRYPOINT ["node", "app.js"]`)
- [ ] C) Raw Form (`ENTRYPOINT: node app.js`)
- [ ] D) Array Form (`ENTRYPOINT ('node', 'app.js')`)

<details>
<summary><b>Explanation</b></summary>
The Exec form (JSON array) runs the binary directly without a wrapping shell, ensuring it becomes PID 1 and receives termination signals.
</details>

---

### 2. Which BuildKit mount type allows mounting secrets securely during build time without baking them into layers?
- [ ] A) `--mount=type=cache`
- [x] B) `--mount=type=secret`
- [ ] C) `--mount=type=bind`
- [ ] D) `--mount=type=tmpfs`

<details>
<summary><b>Explanation</b></summary>
<code>--mount=type=secret</code> mounts secrets into memory temporarily during instruction execution and never writes them to image layers.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
