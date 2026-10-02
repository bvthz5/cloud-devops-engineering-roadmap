# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Run ShellCheck across all scripts
shellcheck -x scripts/*.sh

# Run Bats unit tests
bats test/

# Run pytest with coverage and moto
pytest -v --cov=scripts --cov-report=term-missing

# Run Go tests with race detection
go test -v -race -cover ./...

# Python Pre-Commit Local Run
pre-commit run --all-files
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (09-Kubernetes-Client-Go-and-Custom-Controllers) →](../09-Kubernetes-Client-Go-and-Custom-Controllers/01-Kubernetes-API-Machinery-and-client-go-Architecture.md) |
