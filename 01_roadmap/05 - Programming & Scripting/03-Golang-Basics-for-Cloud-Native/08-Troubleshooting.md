# 08 - Golang for Cloud-Native: Troubleshooting Guide

## 1. Detecting Race Conditions with `-race`

Data races in concurrent Go code corrupt memory unpredictably. Always run tests and builds with the race detector enabled:

```bash
go test -race ./...
go build -race -o my-tool
```

---

## 2. Profiling Memory and CPU with `pprof`

```bash
# Capture 30-second CPU profile of running Go binary
go tool pprof http://localhost:6060/debug/pprof/profile?seconds=30
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
