# 06 - Kubernetes API Interaction with client-go

## 1. Connecting to Kubernetes with `client-go`

`client-go` is the official Go client library used by Kubernetes controllers and operators:

```go
package main

import (
	"context"
	"fmt"

	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// Load kubeconfig from home directory
	config, err := clientcmd.BuildConfigFromFlags("", "/home/user/.kube/config")
	if err != nil {
		panic(err.Error())
	}

	clientset, err := kubernetes.NewForConfig(config)
	if err != nil {
		panic(err.Error())
	}

	// List all Pods in the 'production' namespace
	pods, err := clientset.CoreV1().Pods("production").List(context.TODO(), metav1.ListOptions{})
	if err != nil {
		panic(err.Error())
	}

	fmt.Printf("Found %d pods in production namespace:\n", len(pods.Items))
	for _, pod := range pods.Items {
		fmt.Printf("- %s (Status: %s)\n", pod.Name, pod.Status.Phase)
	}
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - HTTP & Context](./05-HTTP-Services-and-Context-Cancellation.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
