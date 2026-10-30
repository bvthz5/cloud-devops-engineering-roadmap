# Interview Q&A - GitHub Actions Workflows & Runners

> **Module**: GitHub Actions Workflows & Runners

---

### Q1: What is the difference between Continuous Delivery and Continuous Deployment?
**Answer**:
- **Continuous Delivery**: Code changes are automatically built, tested, and staged in pre-production. However, the final deployment to production requires a manual business/operator trigger.
- **Continuous Deployment**: Every code change that passes the automated testing pipeline is automatically deployed directly to production without any manual human intervention.

---

### Q2: How do DORA metrics evaluate DevOps pipeline maturity?
**Answer**:
DORA (DevOps Research and Assessment) measures performance using 4 core metrics:
1. **Deployment Frequency**: How often code is successfully deployed to production.
2. **Lead Time for Changes**: Time taken from code commit to running in production.
3. **Change Failure Rate**: Percentage of production deployments causing degradation/outage.
4. **Failed Service Recovery Time (MTTR)**: Time taken to restore service after a failure occurs.

---

### Q3: Why is Kaniko preferred over Docker-in-Docker for building container images in Kubernetes CI pipelines?
**Answer**:
Docker-in-Docker requires running the build container in **privileged mode** (`--privileged`), which creates severe security vulnerabilities on shared Kubernetes nodes. **Kaniko** builds container images from a Dockerfile inside a container or Kubernetes pod without relying on a Docker daemon or privileged host access.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
