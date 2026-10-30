# 10 - Hands-On Practice Labs

## Lab 1: Live Pod Informer with Rate-Limiting Workqueue

### Objective
Build a lightweight standalone Go application that initializes a `client-go` Informer, watches Pod lifecycle events in the `default` namespace, and enqueues events into a rate-limiting workqueue.

### Implementation
```go
package main

import (
	"fmt"
	"time"

	corev1 "k8s.io/api/core/v1"
	"k8s.io/client-go/informers"
	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/tools/cache"
	"k8s.io/client-go/tools/clientcmd"
	"k8s.io/client-go/util/workqueue"
)

func main() {
	config, _ := clientcmd.BuildConfigFromFlags("", clientcmd.RecommendedHomeFile)
	clientset, _ := kubernetes.NewForConfig(config)

	queue := workqueue.NewTypedRateLimitingQueue[string](
		workqueue.DefaultTypedControllerRateLimiter[string](),
	)
	defer queue.ShutDown()

	factory := informers.NewSharedInformerFactoryWithOptions(
		clientset, 10*time.Minute, informers.WithNamespace("default"),
	)
	podInformer := factory.Core().V1().Pods().Informer()

	podInformer.AddEventHandler(cache.ResourceEventHandlerFuncs{
		AddFunc: func(obj interface{}) {
			key, _ := cache.MetaNamespaceKeyFunc(obj)
			queue.Add(key)
		},
	})

	stopCh := make(chan struct{})
	defer close(stopCh)

	factory.Start(stopCh)
	factory.WaitForCacheSync(stopCh)

	fmt.Println("Worker processing events from workqueue...")
	for {
		key, shutdown := queue.Get()
		if shutdown {
			break
		}
		fmt.Printf("Reconciling key: %s
", key)
		queue.Done(key)
	}
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
