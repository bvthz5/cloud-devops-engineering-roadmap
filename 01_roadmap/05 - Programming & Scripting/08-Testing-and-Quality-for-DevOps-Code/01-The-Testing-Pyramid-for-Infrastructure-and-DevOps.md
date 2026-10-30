# 01 - The Testing Pyramid for Infrastructure and DevOps

## 1. Adapting the Testing Pyramid for Infrastructure Code

In application software, the testing pyramid consists of Unit, Integration, and UI/E2E tests. For DevOps and Cloud Platform code, the pyramid is adapted to balance execution speed, financial cost (cloud resources), and infrastructure fidelity:

```text
               ▲
              / \     End-to-End / Ephemeral Env Tests
             /   \    (Deploy real cloud infrastructure, run smoke tests, destroy)
            /─────\   - Execution: 10 - 45 mins. High cloud cost.
           /       \
          / Integration \ Integration & Emulation Tests
         /  (LocalStack /\ (LocalStack, Testcontainers, ephemeral Docker daemons)
        /  Testcontainers) \ - Execution: 10 - 60 secs. Zero cloud cost.
       /───────────────────\
      /                     \  Unit Tests & Mocking
     /      Unit Tests       \ (pytest + moto, Bats, Go testing, memory mocks)
    /    (pytest, Bats, Go)   \ - Execution: < 5 secs. Zero cloud cost.
   /───────────────────────────\
  /                             \ Static Analysis & Linting
 /   Static Analysis & Linting   \ (ShellCheck, Ruff, Bandit, golangci-lint, PSScriptAnalyzer)
/─────────────────────────────────\ - Execution: Milliseconds. Zero cloud cost.
```

---

## 2. Fast Feedback Loops in CI/CD

Enterprise DevOps pipelines must enforce strict feedback stages:
1. **Pre-Commit / Local Stage**: Formatters, static linters, and fast unit tests execute in under 10 seconds.
2. **Pull Request CI Stage**: Unit tests with 100% mocked cloud calls (`moto`, memory mocks), code coverage checks (minimum 80%), and security scans (`bandit`, `trivy`).
3. **Merge / Nightly Stage**: Integration tests against ephemeral infrastructure (`Testcontainers`, LocalStack, or dedicated sandbox AWS/Azure accounts) with automated teardown.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-Cloud-SDKs-and-Infrastructure-Automation)](../07-Cloud-SDKs-and-Infrastructure-Automation/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Testing Bash Scripts with Bats and ShellCheck →](./02-Testing-Bash-Scripts-with-Bats-and-ShellCheck.md) |
