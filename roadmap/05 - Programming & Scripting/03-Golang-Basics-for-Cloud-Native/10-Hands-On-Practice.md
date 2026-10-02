# 10 - Hands-On Practice: Building a Concurrent Port Scanner in Go

## Lab Scenario
Build a small, concurrent TCP port scanner in Go using Goroutines, Channels, and a WaitGroup.

---

## Lab Steps

### Step 1: Write `scanner.go`
```go
cat << 'EOF' > /tmp/scanner.go
package main

import (
	"fmt"
	"net"
	"sync"
	"time"
)

func scanPort(host string, port int, wg *sync.WaitGroup, results chan<- int) {
	defer wg.Done()
	target := fmt.Sprintf("%s:%d", host, port)
	conn, err := net.DialTimeout("tcp", target, 200*time.Millisecond)
	if err == nil {
		conn.Close()
		results <- port
	}
}

func main() {
	host := "127.0.0.1"
	var wg sync.WaitGroup
	results := make(chan int, 100)

	// Scan common ports concurrently
	ports := []int{22, 80, 443, 3306, 5432, 6379, 8080}
	for _, port := range ports {
		wg.Add(1)
		go scanPort(host, port, &wg, results)
	}

	go func() {
		wg.Wait()
		close(results)
	}()

	fmt.Println("Open ports on", host, ":")
	for port := range results {
		fmt.Printf("- Port %d is OPEN\n", port)
	}
}
EOF
```

### Step 2: Compile and Run
```bash
go run /tmp/scanner.go
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
