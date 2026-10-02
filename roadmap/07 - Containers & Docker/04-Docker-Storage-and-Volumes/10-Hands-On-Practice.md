# 10 - Hands-On Practice Labs

## Lab 1: Persistent Database with Automated Backup Container

### Objective
Launch a persistent PostgreSQL container backed by a named volume, populate data, and take an automated hot backup using an ephemeral Alpine container.

### Implementation
```bash
# 1. Create Volume and Launch DB
docker volume create db_storage
docker run -d --name test_pg -e POSTGRES_PASSWORD=secret -v db_storage:/var/lib/postgresql/data postgres:16-alpine

# Wait for DB initialization
sleep 5

# 2. Populate test table
docker exec -i test_pg psql -U postgres -c "CREATE TABLE users (id serial, name text); INSERT INTO users (name) VALUES ('Alice'), ('Bob');"

# 3. Take automated tar backup
docker run --rm -v db_storage:/data:ro -v "$(pwd)":/backup alpine tar -czf /backup/db_snapshot.tar.gz -C /data .

# 4. Verify archive created
ls -lh db_snapshot.tar.gz
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
