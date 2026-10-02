# 05 - HTTP Services and Context Cancellation

## 1. The Role of `context.Context`

In microservices and API automation, a request must be cancelled if:
1. The client disconnects or closes the connection.
2. The overall operation exceeds its configured deadline/timeout.

```go
package main

import (
	"context"
	"fmt"
	"net/http"
	"time"
)

func fetchClusterMetrics(ctx context.Context) error {
	req, err := http.NewRequestWithContext(ctx, "GET", "https://api.internal/metrics", nil)
	if err != nil {
		return err
	}

	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("request cancelled or failed: %w", err)
	}
	defer resp.Body.Close()
	return nil
}

func main() {
	// Create context with a strict 2-second timeout
	ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
	defer cancel()

	if err := fetchClusterMetrics(ctx); err != nil {
		fmt.Println("Error:", err)
	}
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Building Production CLIs with Cobra and Viper](./04-Building-Production-CLIs-with-Cobra-and-Viper.md) | [Index](../../../README.md) | [06 - Kubernetes API Interaction with client go →](./06-Kubernetes-API-Interaction-with-client-go.md) |
