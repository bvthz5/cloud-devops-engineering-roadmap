# 01 - Kubernetes API Machinery and client-go Architecture

## 1. The Kubernetes API Machinery

The Kubernetes API Server (`kube-apiserver`) is a declarative RESTful HTTP server backed by `etcd`. Every entity is organized hierarchically:
- **Group**: Logical grouping of related capabilities (e.g., `apps`, `batch`, `networking.k8s.io`, or core ``).
- **Version**: API maturity level (`v1alpha1`, `v1beta1`, `v1`).
- **Kind**: The concrete resource schema (e.g., `Deployment`, `Pod`, `Service`).
- **Resource**: The lowercase, plural HTTP endpoint identifier used in REST URLs (e.g., `/apis/apps/v1/namespaces/default/deployments`).

---

## 2. `client-go` Client Types

`k8s.io/client-go` provides four distinct client implementations:

```text
+-------------------------------------------------------------------------------+
|                               k8s.io/client-go                                |
+-----------------------+-------------------------------+-----------------------+
                        |
       ┌────────────────┼───────────────────────────────┐
       ▼                ▼                               ▼
[ Clientset ]     [ DynamicClient ]             [ RESTClient ]
  - Strongly-typed  - Untyped (unstructured.Un-    - Low-level foundation
  - Generated for     structured)                  - Direct HTTP requests
    known built-in  - Essential for arbitrary      - Used internally by
    types (Pod,       CRDs without generating code   Clientset & Dynamic
    Deployment)     - Dynamic runtime schema
```

### Initializing Clientset with In-Cluster or Kubeconfig Auth
```go
package main

import (
	"context"
	"fmt"
	"os"
	"path/filepath"

	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/rest"
	"k8s.io/client-go/tools/clientcmd"
)

func getKubernetesClient() (*kubernetes.Clientset, error) {
	// 1. Try In-Cluster configuration (when running inside a Pod)
	config, err := rest.InClusterConfig()
	if err == nil {
		return kubernetes.NewForConfig(config)
	}

	// 2. Fallback to local ~/.kube/config (for local development)
	home, _ := os.UserHomeDir()
	kubeconfig := filepath.Join(home, ".kube", "config")
	config, err = clientcmd.BuildConfigFromFlags("", kubeconfig)
	if err != nil {
		return nil, fmt.Errorf("unable to load kubeconfig: %w", err)
	}

	// Adjust QPS & Burst for enterprise high throughput
	config.QPS = 50
	config.Burst = 100

	return kubernetes.NewForConfig(config)
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Informers, Listers & Workqueue](./02-Informers-Listers-and-the-Workqueue-Pattern.md) |
