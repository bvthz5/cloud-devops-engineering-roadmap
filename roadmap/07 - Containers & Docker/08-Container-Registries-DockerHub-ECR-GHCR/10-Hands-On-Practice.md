# 10 - Hands-On Practice Labs

## Lab 1: Deploying a Private Local OCI Registry

### Objective
Deploy a private Docker registry container locally, push a tagged image, and inspect its manifest via curl.

### Implementation
```bash
# 1. Start private registry on port 5000
docker run -d -p 5000:5000 --restart=always --name local-registry registry:2

# 2. Tag and push an image
docker pull alpine:latest
docker tag alpine:latest localhost:5000/my-alpine:v1.0
docker push localhost:5000/my-alpine:v1.0

# 3. Query the catalog via Registry HTTP API v2
curl http://localhost:5000/v2/_catalog
# Output: {"repositories":["my-alpine"]}

# 4. View tags
curl http://localhost:5000/v2/my-alpine/tags/list
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
