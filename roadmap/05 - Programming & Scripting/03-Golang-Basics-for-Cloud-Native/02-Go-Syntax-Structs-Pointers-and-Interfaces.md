# 02 - Go Syntax: Structs, Pointers, and Interfaces

## 1. Explicit Error Handling

Go intentionally avoids `try/catch` exceptions. Errors are normal values returned explicitly:

```go
package main

import (
	"fmt"
	"os"
)

func readConfig(path string) ([]byte, error) {
	data, err := os.ReadFile(path)
	if err != nil {
		return nil, fmt.Errorf("failed to read config at %s: %w", path, err)
	}
	return data, nil
}
```

---

## 2. Structs and Interfaces

Interfaces in Go are implemented **implicitly** (duck-typing). If a struct implements the methods defined by an interface, it satisfies that interface automatically:

```go
type CloudDeployer interface {
	Deploy(appName string) error
}

type AWSDeployer struct {
	Region string
}

func (a *AWSDeployer) Deploy(appName string) error {
	fmt.Printf("Deploying %s to AWS region %s\n", appName, a.Region)
	return nil
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Why Go Dominates Cloud Native Infrastructure](./01-Why-Go-Dominates-Cloud-Native-Infrastructure.md) | [Index](../../../README.md) | [03 - Concurrency Goroutines Channels and WaitGroups →](./03-Concurrency-Goroutines-Channels-and-WaitGroups.md) |
