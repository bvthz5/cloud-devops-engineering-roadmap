# 04 - Golang Testing and Testcontainers

## 1. Idiomatic Table-Driven Tests in Go

Go's standard library provides the `testing` package. The standard idiom for unit testing in Cloud Native Go is the **table-driven test**.

```go
package validator

import (
	"testing"
)

func ValidateKubernetesName(name string) bool {
	if len(name) == 0 || len(name) > 63 {
		return false
	}
	// Simplified RFC 1123 DNS Subdomain validation
	for _, ch := range name {
		if !((ch >= 'a' && ch <= 'z') || (ch >= '0' && ch <= '9') || ch == '-') {
			return false
		}
	}
	return true
}

func TestValidateKubernetesName(t *testing.T) {
	tests := []struct {
		name     string
		input    string
		expected bool
	}{
		{"valid lowercase", "my-service-prod", true},
		{"valid alphanumeric", "worker01", true},
		{"invalid uppercase", "MyService", false},
		{"invalid symbols", "service_name", false},
		{"invalid empty", "", false},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			result := ValidateKubernetesName(tt.input)
			if result != tt.expected {
				t.Errorf("ValidateKubernetesName(%q) = %v; want %v", tt.input, result, tt.expected)
			}
		})
	}
}
```

```bash
# Execute tests with race detector and coverage
go test -v -race -cover ./...
```

---

## 2. Integration Testing with `testcontainers-go`

When unit mocks are insufficient (e.g., validating real PostgreSQL transactions, Redis distributed locks, or Kafka messaging), **`testcontainers-go`** manages ephemeral Docker containers directly from Go unit tests and guarantees cleanup via the `Moby/Ryuk` container reaper.

```go
package integration_test

import (
	"context"
	"testing"

	"github.com/redis/go-redis/v9"
	"github.com/testcontainers/testcontainers-go"
	"github.com/testcontainers/testcontainers-go/wait"
)

func TestRedisDistributedLock(t *testing.T) {
	ctx := context.Background()

	// Spin up ephemeral Redis 7 container
	req := testcontainers.ContainerRequest{
		Image:        "redis:7-alpine",
		ExposedPorts: []string{"6379/tcp"},
		WaitingFor:   wait.ForLog("Ready to accept connections"),
	}
	redisC, err := testcontainers.GenericContainer(ctx, testcontainers.GenericContainerRequest{
		ContainerRequest: req,
		Started:          true,
	})
	if err != nil {
		t.Fatalf("Failed to start Redis container: %s", err)
	}
	defer redisC.Terminate(ctx) // Guarantees container destruction on test exit

	endpoint, err := redisC.Endpoint(ctx, "")
	if err != nil {
		t.Fatalf("Failed to get endpoint: %s", err)
	}

	client := redis.NewClient(&redis.Options{Addr: endpoint})
	if err := client.Ping(ctx).Err(); err != nil {
		t.Fatalf("Redis ping failed: %s", err)
	}

	t.Logf("Successfully verified Redis lock on live ephemeral container at %s", endpoint)
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Python Testing with pytest](./03-Python-Unit-and-Integration-Testing-pytest-and-Moto.md) | [README](./README.md) | [05 - Static Analysis & CI Gates](./05-Static-Analysis-Security-Linting-and-CI-Gates.md) |
