# 03 - Kubernetes Automation (`kubernetes.core.k8s`)

```yaml
- name: Deploy Nginx Application on K8s
  kubernetes.core.k8s:
    state: present
    definition:
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: nginx-deployment
        namespace: default
      spec:
        replicas: 3
        template:
          spec:
            containers:
              - name: nginx
                image: nginx:latest
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Provisioning AWS Infrastructure](./02-Provisioning-AWS-Infrastructure-EC2-VPC-S3-RDS.md) | [README](./README.md) | [04 - Ansible Operator SDK](./04-Ansible-Operator-SDK-Building-Kubernetes-Operators.md) |
