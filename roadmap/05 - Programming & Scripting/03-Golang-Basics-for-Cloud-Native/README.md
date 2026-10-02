# Module 03: Golang for Cloud-Native & Platform Engineering

Welcome to **Module 03: Golang for Cloud-Native and Platform Engineering**. Go is the language of cloud infrastructure. Kubernetes, Docker, Terraform, Prometheus, Envoy, and Helm are all written in Go. Understanding Go empowers DevOps engineers to write high-performance CLI tools, custom Kubernetes controllers, and microservices.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Explain why **Go dominates cloud-native infrastructure** (statically compiled single binaries, minimal memory footprint, fast startup).
2. Master Go fundamentals: Packages, structs, interfaces, pointers, and explicit error handling (`if err != nil`).
3. Build concurrent tools using **Goroutines** and **Channels** with proper synchronization (`sync.WaitGroup`, `select`).
4. Build production CLI tools with flags, subcommands, and config files using **`spf13/cobra`** and **`viper`**.
5. Write HTTP servers and API clients with timeouts, context cancellation (`context.Context`), and connection pooling.
6. Understand the basics of **`client-go`** to interact programmatically with the Kubernetes API.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Why Go Dominates Cloud-Native](./01-Why-Go-Dominates-Cloud-Native-Infrastructure.md) | Memory safety, static linking, sub-millisecond startup, zero dependency containers |
| 02 | [Go Syntax, Structs & Interfaces](./02-Go-Syntax-Structs-Pointers-and-Interfaces.md) | Value vs pointer receivers, duck-typing interfaces, explicit error patterns |
| 03 | [Concurrency: Goroutines & Channels](./03-Concurrency-Goroutines-Channels-and-WaitGroups.md) | Lightweight green threads (2KB stack), channel pipelines, preventing race conditions |
| 04 | [Production CLIs with Cobra & Viper](./04-Building-Production-CLIs-with-Cobra-and-Viper.md) | Developing enterprise CLI tools, POSIX flags, environment variable bindings |
| 05 | [HTTP Services & Context Management](./05-HTTP-Services-and-Context-Cancellation.md) | `net/http` client/server, `context.Context` propagation, graceful shutdown |
| 06 | [Kubernetes API with client-go](./06-Kubernetes-API-Interaction-with-client-go.md) | In-cluster vs kubeconfig authentication, Pod listing, basic informer patterns |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Goroutine leak exhausting file descriptors, unbuffered channel deadlock |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Detecting data races with `go test -race`, profiling with `pprof`, build flags |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps Golang interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Building a concurrent URL health checker CLI in Go |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Syntax cheat sheet, context rules, Cobra template, build command reference |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Python for DevOps](../02-Python-for-DevOps-and-Automation/README.md) | [README](./README.md) | [01 - Why Go](./01-Why-Go-Dominates-Cloud-Native-Infrastructure.md) |
