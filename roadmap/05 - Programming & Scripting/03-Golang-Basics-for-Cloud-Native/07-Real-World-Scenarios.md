# 07 - Golang for Cloud-Native: Real-World Production Scenarios

## Scenario 1: Goroutine Leak Exhausts File Descriptors

### Incident Summary
A custom Prometheus exporter written in Go leaked memory and crashed every 48 hours in Kubernetes with:
```
fatal error: runtime: out of memory
socket: too many open files
```

### Root Cause
Inside an HTTP polling loop, the developer forgot to read and close the HTTP response body:
```go
resp, err := client.Do(req)
// Missing: defer resp.Body.Close()
```
Because the HTTP response body was never drained or closed, the underlying TCP connection remained open in memory. Over 48 hours, 65,000 abandoned sockets accumulated until the OS limit was reached!

### Resolution
Always close response bodies immediately after checking for errors:
```go
resp, err := client.Do(req)
if err != nil { return err }
defer resp.Body.Close()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Kubernetes API Interaction with client go](./06-Kubernetes-API-Interaction-with-client-go.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
