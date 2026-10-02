# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which condition ensures that Docker Compose waits for a container's healthcheck to pass before starting dependent services?
- [ ] A) `condition: service_started`
- [x] B) `condition: service_healthy`
- [ ] C) `condition: service_ready`
- [ ] D) `condition: healthcheck_ok`

<details>
<summary><b>Explanation</b></summary>
<code>condition: service_healthy</code> instructs Compose to wait for the healthcheck test to report healthy status.
</details>

---

### 2. Which command outputs the final merged Compose YAML after evaluating all overrides and environment variables?
- [ ] A) `docker compose dump`
- [x] B) `docker compose config`
- [ ] C) `docker compose validate`
- [ ] D) `docker compose inspect`

<details>
<summary><b>Explanation</b></summary>
<code>docker compose config</code> validates and prints the fully resolved, merged Compose configuration.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
