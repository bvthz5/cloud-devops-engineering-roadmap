# 01 - Horizontal Pod Autoscaler (HPA v2) and Metrics APIs

## 1. How HPA Calculates Desired Replicas

The **Horizontal Pod Autoscaler (HPA)** automatically scales the number of Pod replicas in a Deployment or StatefulSet based on observed resource utilization.

### The Mathematical Formula

$$	ext{DesiredReplicas} = \left\lceil 	ext{CurrentReplicas} 	imes \left( rac{	ext{CurrentMetricValue}}{	ext{TargetMetricValue}} ight) ightceil$$

Example:
- `CurrentReplicas`: 4
- `CurrentCPU`: 80m
- `TargetCPU`: 50m
$$	ext{DesiredReplicas} = \left\lceil 4 	imes \left(rac{80}{50}ight) ightceil = \lceil 4 	imes 1.6 ceil = \lceil 6.4 ceil = 7 	ext{ Replicas}$$

---

## 2. Production HPA v2 Manifest

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-autoscaler
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api-service
  minReplicas: 3
  maxReplicas: 30
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: AverageValue
        averageValue: 400Mi
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300   # Prevent flapping: wait 5m before scaling down!
      policies:
      - type: Percent
        value: 10
        periodSeconds: 60
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Custom Metrics](./02-Custom-and-External-Metrics-with-Prometheus-Adapter.md) |
