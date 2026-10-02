# 10 - Hands-On Practice Labs

## Lab 1: Isolated Multi-Tier Network with DNS Resolution

### Objective
Create two separate user-defined bridge networks (`frontend_net` and `backend_net`), deploy a 3-container topology, and verify that the frontend cannot reach the database directly.

### Implementation
```bash
# 1. Create isolated networks
docker network create frontend_net
docker network create backend_net

# 2. Deploy database on backend_net
docker run -d --name db --net backend_net postgres:alpine

# 3. Deploy API connected to BOTH networks (Gateway bridge)
docker run -d --name api --net frontend_net alpine sleep 3600
docker network connect backend_net api

# 4. Deploy web frontend on frontend_net ONLY
docker run -d --name web --net frontend_net alpine sleep 3600

# 5. Verify connectivity:
# API can reach DB:
docker exec api ping -c 2 db
# Web CANNOT reach DB (Isolation success!):
docker exec web ping -c 2 db || echo "Isolated successfully!"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
