# Deploying to Kubernetes with GitLab Agent for Kubernetes

> **Module**: GitLab CI Pipelines & Runners  
> **Level**: Production & Deep-Dive  

---

## 📌 Executive Summary
Mastering **Deploying to Kubernetes with GitLab Agent for Kubernetes** is fundamental for building reliable, fast, and secure Continuous Integration and Continuous Deployment (CI/CD) pipelines. This guide covers pipeline mechanics, runner executor configurations, caching strategies, and enterprise deployment guardrails.

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
1. **Pipeline-as-Code**: Storing build, test, and release definitions alongside code in version control (`.github/workflows`, `.gitlab-ci.yml`, `Jenkinsfile`).
2. **Ephemeral Runners**: Utilizing on-demand Docker/Kubernetes pod runners to ensure isolated, clean build environments.
3. **Artifact Management**: Passing intermediate build outputs and container images securely between job stages.
4. **Shift-Left Security**: Running unit tests, linters, and vulnerability scanners automatically on every pull request.

---

## 🛠 YAML & Pipeline Examples

### GitHub Actions Workflow (.github/workflows/ci.yml)
```yaml
name: CI/CD Production Pipeline
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies & Test
        run: |
          npm ci
          npm test
```

### Declarative Jenkinsfile Syntax
```groovy
pipeline {
    agent { label 'kubernetes' }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```

---

## 💡 Production Best Practices
- **Never Store Secrets in Code**: Access secrets via native runner secrets store, HashiCorp Vault, or OIDC dynamic tokens.
- **Leverage Dependency Caching**: Cache `~/.m2`, `node_modules`, or `~/.cache/go-build` to accelerate build times by up to 70%.
- **Fail Fast**: Position fast static analysis and unit tests early in the pipeline before executing heavy integration or end-to-end test suites.
- **Track DORA Metrics**: Monitor Deployment Frequency, Lead Time for Changes, Mean Time to Recovery (MTTR), and Change Failure Rate.

---

## 🔗 Related Resources
- [GitHub Actions Official Documentation](https://docs.github.com/en/actions)
- [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- [Jenkins User Handbook](https://www.jenkins.io/doc/book/)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Auto DevOps Templates and Component Catalog](./05-Auto-DevOps-Templates-and-Component-Catalog.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
