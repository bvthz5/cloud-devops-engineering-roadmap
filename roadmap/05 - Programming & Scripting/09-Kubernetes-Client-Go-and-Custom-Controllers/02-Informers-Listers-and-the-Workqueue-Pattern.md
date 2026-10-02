# 02 - Informers, Listers, and the Workqueue Pattern

## 1. Why Direct API Polling Is Prohibited

If 100 controllers constantly polled the `kube-apiserver` with `List(Pod)` every 5 seconds, `etcd` and the API server would collapse under memory and CPU exhaustion.

The Kubernetes Controller pattern solves this with **Informers** and **Listers**:
1. An **Informer** opens a long-lived HTTP streaming connection (`Watch`) to the API server.
2. It caches the latest cluster state in an in-memory thread-safe store (**Indexer**).
3. The controller queries the **Lister** (which reads from local RAM cache), resulting in **zero** network calls to the API server for read queries!

```text
[ kube-apiserver ]
        │  (Watch HTTP Stream: HTTP Chunked / Protobuf)
        ▼
   [ Reflector ]
        │
        ▼
   [ DeltaFIFO ]
        │
        ├──► [ Indexer Cache (RAM) ] ◄── (Lister queries read here!)
        │
        ▼
 [ ResourceEventHandler ] (AddFunc, UpdateFunc, DeleteFunc)
        │
        ▼ (Pushes Key: "namespace/name")
   [ Workqueue ] (Rate-limiting queue)
        │
        ▼
 [ Reconcile Worker Loop ]
```

---

## 2. Implementing a SharedInformerFactory

```go
package main

import (
	"fmt"
	"time"

	corev1 "k8s.io/api/core/v1"
	"k8s.io/client-go/informers"
	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/tools/cache"
)

func StartPodInformer(clientset *kubernetes.Clientset, stopCh <-chan struct{}) {
	// Resync period: re-evaluates all items periodically
	factory := informers.NewSharedInformerFactory(clientset, 10*time.Minute)
	podInformer := factory.Core().V1().Pods().Informer()

	podInformer.AddEventHandler(cache.ResourceEventHandlerFuncs{
		AddFunc: func(obj interface{}) {
			pod := obj.(*corev1.Pod)
			fmt.Printf("[ADD] Pod spawned: %s/%s
", pod.Namespace, pod.Name)
		},
		UpdateFunc: func(oldObj, newObj interface{}) {
			newPod := newObj.(*corev1.Pod)
			fmt.Printf("[UPDATE] Pod updated: %s/%s (Status: %s)
", newPod.Namespace, newPod.Name, newPod.Status.Phase)
		},
		DeleteFunc: func(obj interface{}) {
			pod := obj.(*corev1.Pod)
			fmt.Printf("[DELETE] Pod deleted: %s/%s
", pod.Namespace, pod.Name)
		},
	})

	factory.Start(stopCh)
	factory.WaitForCacheSync(stopCh)
	fmt.Println("Informer cache successfully synced with apiserver.")
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Kubernetes API Machinery](./01-Kubernetes-API-Machinery-and-client-go-Architecture.md) | [README](./README.md) | [03 - Custom Resource Definitions](./03-Custom-Resource-Definitions-CRDs-and-Code-Generation.md) |
