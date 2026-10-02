# 11 - Multiple-Choice Assessment (MCQ)

### 1. In Traefik, which component is responsible for modifying requests (e.g. rate limiting, auth, header manipulation)?
- [ ] A) EntryPoint
- [ ] B) Service
- [x] C) Middleware
- [ ] D) Provider

<details>
<summary><b>Explanation</b></summary>
Middlewares sit between Routers and Services, modifying incoming requests (or outgoing responses) before forwarding.
</details>

---

### 2. What strict file permission is mandated by Traefik on the `acme.json` storage file?
- [ ] A) 0777
- [ ] B) 0644
- [x] C) 0600
- [ ] D) 0755

<details>
<summary><b>Explanation</b></summary>
Traefik mandates 0600 (read/write for owner only) on <code>acme.json</code> to prevent unauthorized processes from reading private keys.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
