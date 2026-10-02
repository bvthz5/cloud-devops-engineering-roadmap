# 02 - Custom and External Metrics with Prometheus Adapter

## 1. Scaling Beyond CPU and Memory

CPU and memory are lagging indicators. For event-driven microservices, scaling on **incoming HTTP request rates** or **queue backlog** prevents latency spikes:

```yaml
spec:
  metrics:
  - type: External
    external:
      metric:
        name: sqs_messages_visible
        selector:
          matchLabels:
            queue: order-processing
      target:
        type: AverageValue
        averageValue: 30        # Add 1 pod for every 30 messages in the queue!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - HPA v2 Architecture](./01-Horizontal-Pod-Autoscaler-HPA-v2-and-Metrics-APIs.md) | [README](./README.md) | [03 - Vertical Pod Autoscaler](./03-Vertical-Pod-Autoscaler-VPA-Modes-and-Recommender.md) |
