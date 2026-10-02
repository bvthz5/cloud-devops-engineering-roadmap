# 07 — Real-World Production Scenarios: Encoding & Binary

---

## Scenario 1: The Trailing Newline in Base64 Kubernetes Secret

### Incident Summary
A CI/CD pipeline deploys a microservice to Kubernetes. The database connection fails: `FATAL: password authentication failed for user "app"`.
The developer verifies the password string in the pipeline and confirms it matches the database password exactly.

### Root Cause Analysis
The pipeline generated the secret using:
`echo "db_pass" | base64`
`echo` appended a trailing newline `0x0A`. The resulting Base64 string encoded `db_pass
`. The application sent the password with the trailing newline, which PostgreSQL rejected!

### Production Solution
Always use `echo -n` or `printf`:
```bash
printf '%s' "$DB_PASSWORD" | base64
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Data Encoding Standards Base64 Hex URL Encoding](./06-Data-Encoding-Standards-Base64-Hex-URL-Encoding.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
