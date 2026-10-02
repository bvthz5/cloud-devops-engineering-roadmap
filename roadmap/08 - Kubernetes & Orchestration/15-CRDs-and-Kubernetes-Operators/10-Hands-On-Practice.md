# 10 - Hands-On Practice Labs

## Lab: Registering a Custom CRD and Verifying Schema Validation

```bash
# 1. Create a Custom Resource Definition
cat << 'EOF' | kubectl apply -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: apis.cloud.example.com
spec:
  group: cloud.example.com
  names:
    kind: ApiGateway
    plural: apis
    singular: api
    shortNames: ["agw"]
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            required: ["rateLimit"]
            properties:
              rateLimit:
                type: integer
                minimum: 10
EOF

# 2. Create valid custom resource instance
cat << 'EOF' | kubectl apply -f -
apiVersion: cloud.example.com/v1
kind: ApiGateway
metadata:
  name: prod-gateway
spec:
  rateLimit: 100
EOF

# 3. Verify object creation
kubectl get agw
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
