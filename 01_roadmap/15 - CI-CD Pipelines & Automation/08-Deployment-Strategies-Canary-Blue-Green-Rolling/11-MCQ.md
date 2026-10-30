# MCQ - Deployment Strategies (Canary, Blue-Green, Rolling)

> **Module**: Deployment Strategies (Canary, Blue-Green, Rolling)

---

### Question 1
Which deployment strategy involves routing a small percentage of live user traffic (e.g., 5%) to a new software release while monitoring latency and error metrics?
- [ ] A) Recreate Deployment
- [x] B) Canary Deployment
- [ ] C) Blue-Green Deployment
- [ ] D) Hard Cutover

*Explanation: Canary deployments route small percentages of live traffic to test new releases in production safely.*

---

### Question 2
What is the primary security benefit of using Multi-Stage Docker builds?
- [ ] A) It automatically rotates passwords in database connections
- [x] B) It leaves compilers, SDKs, and build tools out of the final runtime image, shrinking the attack surface
- [ ] C) It encrypts container logs automatically
- [ ] D) It bypasses image vulnerability scanning

*Explanation: Multi-stage builds produce minimal production runtime images without bloated build tools.*

---

### Question 3
Which database migration pattern allows deploying new application code without breaking older running application instances?
- [ ] A) Drop-Create Pattern
- [ ] B) Single-Transaction Pattern
- [x] C) Expand-Contract Pattern
- [ ] D) Lock-Table Pattern

*Explanation: Expand-Contract introduces schema changes incrementally to ensure backward compatibility during deployments.*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
