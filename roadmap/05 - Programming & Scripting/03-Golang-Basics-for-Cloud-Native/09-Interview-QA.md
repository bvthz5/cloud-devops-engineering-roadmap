# 09 - Golang for Cloud-Native: Interview Questions & Answers

### Q1: Why are Goroutines significantly lighter than operating system threads?
**Answer:** An OS thread typically reserves 1 to 2 Megabytes of memory for its execution stack and requires expensive kernel-space context switching. A Goroutine is managed in user space by the Go runtime scheduler (GMP model), starts with a tiny **2 Kilobyte stack** that grows and shrinks dynamically, allowing a single Go program to run hundreds of thousands of concurrent Goroutines effortlessly.

### Q2: What is the purpose of `context.Context` in Go cloud services?
**Answer:** `context.Context` carries deadlines, cancellation signals, and request-scoped values across API boundaries and goroutine trees. It ensures that if a client disconnects or an operation times out, all downstream database queries and HTTP calls are cancelled immediately to prevent resource waste.

### Q3: What is the difference between a buffered and an unbuffered channel in Go?
**Answer:** An unbuffered channel (`make(chan int)`) has zero capacity; a send operation blocks until another goroutine executes a receive, facilitating synchronous rendezvous communication. A buffered channel (`make(chan int, 100)`) can accept sends without blocking until the buffer is full.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
