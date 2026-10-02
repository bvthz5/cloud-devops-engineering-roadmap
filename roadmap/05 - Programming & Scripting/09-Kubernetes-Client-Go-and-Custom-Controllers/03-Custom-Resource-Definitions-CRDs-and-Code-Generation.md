# 03 - Custom Resource Definitions (CRDs) and Code Generation

## 1. Custom Resource Definitions (CRDs)

A CRD extends the Kubernetes API with bespoke domain models. For example, creating a custom `DatabaseInstance` resource:

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databaseinstances.database.example.com
spec:
  group: database.example.com
  names:
    kind: DatabaseInstance
    plural: databaseinstances
    singular: databaseinstance
    shortNames:
      - dbi
  scope: Namespaced
  versions:
    - name: v1alpha1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              required: ["engine", "storageGB"]
              properties:
                engine:
                  type: string
                  enum: ["postgres", "mysql"]
                storageGB:
                  type: integer
                  minimum: 10
            status:
              type: object
              properties:
                phase:
                  type: string
                endpoint:
                  type: string
      subresources:
        status: {}
```

---

## 2. Code-Generator Tags (`k8s.io/code-generator`)

When developing native Go operators, developers define Go structs with code-generator comments to automatically generate deepcopy routines, typed clientsets, listers, and informers:

```go
package v1alpha1

import metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"

// +genclient
// +k8s:deepcopy-gen:interfaces=k8s.io/apimachinery/pkg/runtime.Object

// DatabaseInstance is the Schema for the databaseinstances API
type DatabaseInstance struct {
	metav1.TypeMeta   `json:",inline"`
	metav1.ObjectMeta `json:"metadata,omitempty"`

	Spec   DatabaseInstanceSpec   `json:"spec,omitempty"`
	Status DatabaseInstanceStatus `json:"status,omitempty"`
}

type DatabaseInstanceSpec struct {
	Engine    string `json:"engine"`
	StorageGB int    `json:"storageGB"`
}

type DatabaseInstanceStatus struct {
	Phase    string `json:"phase,omitempty"`
	Endpoint string `json:"endpoint,omitempty"`
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Informers, Listers & Workqueue](./02-Informers-Listers-and-the-Workqueue-Pattern.md) | [README](./README.md) | [04 - Writing a Controller Reconcile Loop](./04-Writing-a-Kubernetes-Controller-The-Reconcile-Loop.md) |
