# 05 - AWX REST API & Webhooks

Launch job template via REST API:
```bash
curl -X POST https://awx.example.com/api/v2/job_templates/42/launch/ \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - RBAC Organizations Teams and Access Control](./04-RBAC-Organizations-Teams-and-Access-Control.md) | [Index](../../../README.md) | [06 - Credential Management and Secret Stores in AWX →](./06-Credential-Management-and-Secret-Stores-in-AWX.md) |
