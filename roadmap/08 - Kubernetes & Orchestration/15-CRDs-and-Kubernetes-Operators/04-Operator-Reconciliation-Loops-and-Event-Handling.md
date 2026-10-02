# 04 - Operator Reconciliation Loops and Event Handling

## 1. Anatomy of the `Reconcile` Function

```go
func (r *DatabaseReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
    // 1. Fetch custom resource from API Server
    var db appsv1.Database
    if err := r.Get(ctx, req.NamespacedName, &db); err != nil {
        return ctrl.Result{}, client.IgnoreNotFound(err)
    }

    // 2. Check if desired child Deployment exists
    var dep appsv1.Deployment
    err := r.Get(ctx, types.NamespacedName{Name: db.Name, Namespace: db.Namespace}, &dep)
    if errors.IsNotFound(err) {
        // Create child deployment
        newDep := r.constructDeployment(&db)
        if err := r.Create(ctx, newDep); err != nil {
            return ctrl.Result{}, err
        }
    }

    // 3. Return success (no requeue needed until next event)
    return ctrl.Result{}, nil
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Kubebuilder Framework](./03-Building-Operators-with-Kubebuilder-and-Controller-Runtime.md) | [README](./README.md) | [05 - Finalizers & Teardown](./05-Finalizers-and-Safe-Resource-Teardown.md) |
