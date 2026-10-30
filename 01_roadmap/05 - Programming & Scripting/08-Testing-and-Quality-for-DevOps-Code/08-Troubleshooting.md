# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Debugging Flaky Integration Tests in CI

```text
Problem: Test Passes Locally but Fails Intermittently in CI Runners
  │
  ├──► Is it a Timezone / Date Flake?
  │      └── Ensure tests use explicit UTC timestamps: datetime.now(timezone.utc)
  │
  ├──► Is it an Asynchronous / Race Condition?
  │      ├── Run Go tests with: go test -race -count=10 ./...
  │      └── Replace hardcoded time.Sleep() / sleep 5 with deterministic polling loops.
  │
  └──► Is it a Shared State / Resource Leak?
         ├── Are Docker testcontainers failing to terminate? Check ryuk container logs.
         └── Ensure test teardowns execute in finally / defer / teardown() handlers.
```

---

## 2. Resolving Docker-in-Docker Socket Permissions for Testcontainers

When running `testcontainers-go` or `testcontainers-python` inside a GitLab CI or GitHub Actions container runner, the test runner must mount the Docker socket:

```yaml
# GitHub Actions runner configuration
jobs:
  integration-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Go Integration Tests with Testcontainers
        run: go test -v ./...
        env:
          # Testcontainers automatically detects the host Docker daemon
          TESTCONTAINERS_RYUK_DISABLED: "false"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
