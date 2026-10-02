# 05 - Operator SDK and Kubebuilder Framework

## 1. Kubebuilder Scaffolding and Architecture

Writing boilerplate code for client-go, informers, workqueues, and metrics from scratch is tedious and error-prone. **Kubebuilder** is the official SIG-API-Machinery framework for building production operators using `controller-runtime`.

```bash
# Initialize a new operator project
kubebuilder init --domain example.com --repo example.com/my-operator

# Create API schema and controller scaffolding
kubebuilder create api --group apps --version v1alpha1 --kind WebApp
```

---

## 2. RBAC Markers and Manifest Generation

Kubebuilder uses Go comments (markers) to automatically generate RBAC ClusterRoles, preventing authorization drift:

```go
// +kubebuilder:rbac:groups=apps.example.com,resources=webapps,verbs=get;list;watch;create;update;patch;delete
// +kubebuilder:rbac:groups=apps.example.com,resources=webapps/status,verbs=get;update;patch
// +kubebuilder:rbac:groups=apps.example.com,resources=webapps/finalizers,verbs=update
// +kubebuilder:rbac:groups=apps,resources=deployments,verbs=get;list;watch;create;update;patch;delete
// +kubebuilder:rbac:groups=core,resources=services,verbs=get;list;watch;create;update;patch;delete

func (r *WebAppReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
    // Controller logic here
    return ctrl.Result{}, nil
}
```

```bash
# Generate CRD YAML and RBAC manifests via controller-gen
make manifests
```

---

## 3. Admission Webhooks: Mutating and Validating

Beyond reconciliation, operators frequently inject mutating defaults or enforce strict validation before objects are committed to `etcd`:

```text
[ kubectl apply ] ---> [ API Handler ]
                             │
                             ▼
                 [ Mutating Admission Webhook ] (Injects sidecars / default storage)
                             │
                             ▼
                [ Schema Validation (OpenAPI) ]
                             │
                             ▼
                [ Validating Admission Webhook ] (Rejects unauthorized/invalid configurations)
                             │
                             ▼
                     [ Commit to etcd ]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Writing a Controller Reconcile Loop](./04-Writing-a-Kubernetes-Controller-The-Reconcile-Loop.md) | [README](./README.md) | [06 - Building CLIs with Cobra and Viper](./06-Building-Enterprise-CLIs-with-Cobra-and-Viper.md) |
