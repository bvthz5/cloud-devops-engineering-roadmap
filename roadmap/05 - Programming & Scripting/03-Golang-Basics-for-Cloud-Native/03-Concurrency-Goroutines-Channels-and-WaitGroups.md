# 03 - Concurrency: Goroutines, Channels, and WaitGroups

## 1. Concurrent Worker Pool Pattern

In DevOps automation, you frequently need to check health across 500 servers simultaneously:

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func checkHost(host string, wg *sync.WaitGroup, results chan<- string) {
	defer wg.Done()
	// Simulate network probe
	time.Sleep(100 * time.Millisecond)
	results <- fmt.Sprintf("Host %s: HEALTHY", host)
}

func main() {
	hosts := []string{"web1", "web2", "web3", "db1", "cache1"}
	var wg sync.WaitGroup
	results := make(chan string, len(hosts))

	for _, host := range hosts {
		wg.Add(1)
		go checkHost(host, &wg, results)
	}

	wg.Wait()
	close(results)

	for res := range results {
		fmt.Println(res)
	}
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Go Syntax Structs Pointers and Interfaces](./02-Go-Syntax-Structs-Pointers-and-Interfaces.md) | [Index](../../../README.md) | [04 - Building Production CLIs with Cobra and Viper →](./04-Building-Production-CLIs-with-Cobra-and-Viper.md) |
