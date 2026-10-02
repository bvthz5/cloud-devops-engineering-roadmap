# Interview Q&A - Deployment Strategies (Canary, Blue-Green, Rolling)

> **Module**: Deployment Strategies (Canary, Blue-Green, Rolling)

---

### Q1: What is the Expand-Contract pattern in database migration for zero-downtime CI/CD?
**Answer**:
The Expand-Contract pattern breaks breaking database changes into multi-step, backward-compatible migrations:
1. **Expand**: Add new columns or tables alongside existing ones without modifying old code.
2. **Transition**: Deploy application code that reads from both old and new schemas and writes to both.
3. **Contract**: Once all legacy code is retired and data is migrated, drop the old columns or tables in a final migration step.

---

### Q2: What is the difference between Blue-Green Deployment and Canary Deployment?
**Answer**:
- **Blue-Green**: Provisions two complete identical environments (Blue and Green). All traffic is switched instantly from Blue to Green once testing passes.
- **Canary**: Gradually shifts a small percentage of production traffic (e.g., 5% -> 25% -> 100%) to a new version, monitoring real-user metrics (error rates, latency) before completing full rollout.

---

### Q3: Why are Multi-Stage Docker builds critical in enterprise CI/CD pipelines?
**Answer**:
Multi-stage builds allow separating build-time dependencies (compilers, SDKs, dev dependencies) from runtime artifacts. The final production image copies only compiled binaries or dist folders into a minimal base image (like Alpine or Distroless), reducing image size by up to 90% and drastically shrinking the security attack surface.
