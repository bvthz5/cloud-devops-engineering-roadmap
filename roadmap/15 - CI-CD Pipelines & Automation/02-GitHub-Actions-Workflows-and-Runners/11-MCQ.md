# MCQ - GitHub Actions Workflows & Runners

> **Module**: GitHub Actions Workflows & Runners

---

### Question 1
Which DORA metric measures the time elapsed from a developer committing code to that code successfully executing in production?
- [ ] A) Deployment Frequency
- [x] B) Lead Time for Changes
- [ ] C) Change Failure Rate
- [ ] D) Mean Time to Recovery (MTTR)

*Explanation: Lead Time for Changes measures the speed of code pipeline delivery from commit to production.*

---

### Question 2
What is the key advantage of declarative pipeline syntax over scripted pipeline syntax in CI/CD?
- [ ] A) Scripted pipelines execute 10x faster
- [x] B) Declarative pipelines enforce a standard, structured schema that is easier to read and maintain
- [ ] C) Declarative pipelines do not support caching
- [ ] D) Scripted pipelines cannot interact with Git repositories

*Explanation: Declarative syntax provides a structured, standardized format with built-in validation.*

---

### Question 3
Why should Kaniko or Buildah be used for container builds inside Kubernetes CI agents instead of standard `docker build`?
- [ ] A) They compress images into `.zip` files
- [x] B) They build container images without requiring privileged root access to a Docker daemon
- [ ] C) They do not require a Dockerfile
- [ ] D) They only run on Windows OS

*Explanation: Kaniko executes unprivileged container image builds safely inside Kubernetes pods.*
