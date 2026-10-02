# Static Code Analysis: SonarQube, ESLint and Quality Gates

> **Module**: Automated Testing Pipelines  
> **Level**: Production & Deep-Dive  

---

## 📌 Executive Summary
Mastering **Static Code Analysis: SonarQube, ESLint and Quality Gates** is fundamental for building reliable, fast, and secure Continuous Integration and Continuous Deployment (CI/CD) pipelines. This guide covers pipeline mechanics, runner executor configurations, caching strategies, and enterprise deployment guardrails.

---

## 🏗 Architecture & Execution Flow

```
+-------------------------------------------------------------------------------+
|                        MODERN CI/CD AUTOMATION PIPELINE                      |
+-------------------------------------------------------------------------------+
|  Git Push / PR  -->  Trigger & Webhook  -->  Runner Allocation (K8s/VM)     |
|  Build & Test   -->  SAST/SCA Scan     -->  Artifact Publish  --> Deploy     |
+-------------------------------------------------------------------------------+
```

### Key Concepts
1. **Multi-Stage Docker Builds**: Minimizing container footprint and eliminating build toolchain vulnerabilities in runtime images.
2. **Enterprise Artifact Repositories**: Centralizing Java JARs, npm packages, Docker images, and Helm charts using Artifactory or Nexus with proxy caching.
3. **Automated Testing Pyramid**: Balancing fast unit tests, integration tests with Testcontainers, and end-to-end (E2E) UI testing.
4. **Zero-Downtime Deployment Patterns**: Executing Blue-Green switchovers and progressive Canary releases with automated metrics rollback.

---

## 🛠 Multi-Stage Dockerfile & Kubernetes Deployment Examples

### Multi-Stage Dockerfile Optimization
```dockerfile
# Build Stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Runtime Stage (Minimal production image)
FROM node:18-alpine AS runner
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
USER node
EXPOSE 3000
CMD ["node", "dist/main.js"]
```

### Kubernetes Canary Deployment via Argo Rollouts
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: app-rollout
spec:
  replicas: 5
  strategy:
    canary:
      steps:
      - setWeight: 20
      - pause: { duration: 10m }
      - setWeight: 50
      - pause: { duration: 10m }
```

---

## 💡 Production Best Practices
- **Use Multi-Stage Builds**: Keep production container images lean (under 100MB) by discarding compilers, build tools, and dev dependencies.
- **Proxy External Dependencies**: Configure Nexus or Artifactory as proxy mirrors to protect against public registry outages and rate limits.
- **Enforce Expand-Contract Database Schema Changes**: Ensure database schema migrations (adding columns/tables) are backward-compatible before rolling out new code.
- **Automate Quality Gates**: Block PR merges if SonarQube static code analysis fails code coverage (e.g., < 80%) or uncovers security vulnerabilities.

---

## 🔗 Related Resources
- [Docker Multi-Stage Build Guide](https://docs.docker.com/build/building/multi-stage/)
- [Argo Rollouts Documentation](https://argoproj.github.io/argo-rollouts/)
