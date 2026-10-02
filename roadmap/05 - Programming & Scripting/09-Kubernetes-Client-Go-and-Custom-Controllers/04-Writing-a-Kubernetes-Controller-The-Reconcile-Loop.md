# 04 - Writing a Kubernetes Controller: The Reconcile Loop

## 1. Level-Triggered State Reconciliation

Kubernetes controllers are **level-triggered**, not edge-triggered.
- **Edge-triggered**: Reacts solely to a change event (e.g., "Pod X was deleted"). If the controller was offline during the event, state drifts forever.
- **Level-triggered**: Looks at the **entire current state** and compares it against the **desired state**, driving the system toward convergence regardless of how many intermediate events were missed.

```text
+-----------------------------------------------------------+
|                      Reconcile Loop                       |
+-----------------------------------------------------------+
                             │
                             ▼
     [ 1. Fetch Resource from Lister Cache via Key ]
                             │
                             ├──── If Not Found: Handled by deletion logic
                             ▼
  [ 2. Check DeletionTimestamp (Is object being deleted?) ]
                             │
                             ├──── Yes: Execute cleanup & Remove Finalizer
                             ▼
     [ 3. Compare Current State vs Desired State (Spec) ]
                             │
                             ├── Current == Desired: Return reconcile.Result{} (Done)
                             ▼
         [ 4. Execute Mutating Actions to Converge ]
     (Create child Pod, provision cloud resource, update ConfigMap)
                             │
                             ▼
         [ 5. Update Status Subresource in Apiserver ]
```

---

## 2. Reconcile Function Implementation

```go
package controller

import (
	"context"
	"fmt"
	"time"

	apierrors "k8s.io/apimachinery/pkg/api/errors"
	"k8s.io/apimachinery/pkg/runtime"
	ctrl "sigs.k8s.io/controller-runtime"
	"sigs.k8s.io/controller-runtime/pkg/client"

	dbv1alpha1 "example.com/api/v1alpha1"
)

type DatabaseReconciler struct {
	client.Client
	Scheme *runtime.Scheme
}

func (r *DatabaseReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
	// 1. Fetch current object
	var db dbv1alpha1.DatabaseInstance
	if err := r.Get(ctx, req.NamespacedName, &db); err != nil {
		if apierrors.IsNotFound(err) {
			// Object was deleted; child resources automatically cleaned via Garbage Collection
			return ctrl.Result{}, nil
		}
		return ctrl.Result{}, err
	}

	// 2. Business logic reconciliation
	if db.Status.Phase == "" {
		db.Status.Phase = "Provisioning"
		if err := r.Status().Update(ctx, &db); err != nil {
			return ctrl.Result{}, err
		}
		// Requeue after 5 seconds to check provisioning progress
		return ctrl.Result{RequeueAfter: 5 * time.Second}, nil
	}

	return ctrl.Result{}, nil
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Custom Resource Definitions](./03-Custom-Resource-Definitions-CRDs-and-Code-Generation.md) | [README](./README.md) | [05 - Operator SDK & Kubebuilder](./05-Operator-SDK-and-Kubebuilder-Framework.md) |
