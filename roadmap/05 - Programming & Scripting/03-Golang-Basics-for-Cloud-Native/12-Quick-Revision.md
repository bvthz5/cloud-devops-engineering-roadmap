# 12 - Golang for Cloud-Native: Quick Revision Cheat Sheet

## Concurrency Template
```go
var wg sync.WaitGroup
results := make(chan string, 10)

for _, item := range items {
    wg.Add(1)
    go func(val string) {
        defer wg.Done()
        results <- process(val)
    }(item)
}

go func() {
    wg.Wait()
    close(results)
}()
```

## Compilation Flags for Production
```bash
# Build static binary without CGO, stripped of debug symbols:
CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o myapp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (04-APIs-REST-gRPC-and-Webhooks) →](../04-APIs-REST-gRPC-and-Webhooks/01-REST-API-Architecture-and-Idempotency.md) |
