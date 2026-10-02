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
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [04 - APIs & Webhooks](../04-APIs-REST-gRPC-and-Webhooks/README.md) |
