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
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [09 - Kubernetes Client-Go & Custom Controllers](../09-Kubernetes-Client-Go-and-Custom-Controllers/README.md) |
