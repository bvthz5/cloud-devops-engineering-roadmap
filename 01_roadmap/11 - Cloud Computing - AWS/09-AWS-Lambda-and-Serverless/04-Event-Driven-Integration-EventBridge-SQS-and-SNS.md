# 04 - Event-Driven Architecture: EventBridge vs. SQS vs. SNS

> **Focus:** Loose coupling in distributed cloud applications, Amazon EventBridge (Event Bus), Amazon SQS (Queues & Dead-Letter Queues), and Amazon SNS (Pub/Sub Fanout).

---

## ⚖️ 1. EventBridge vs. SQS vs. SNS Master Comparison

```text
┌───────────────────────────────┐  ┌───────────────────────────────┐  ┌───────────────────────────────┐
│     Amazon SQS (Queues)       │  │       Amazon SNS (Pub/Sub)    │  │ Amazon EventBridge (Event Bus)│
│                               │  │                               │  │                               │
│  • PULL model (Consumer polls)│  │  • PUSH model (Fanout topics) │  │  • JSON schema event routing  │
│  • Buffers messages           │  │  • 1 publisher ➔ 1M subscribers│ • Connects SaaS (Stripe, Datadog│
│  • Standard vs FIFO queues    │  │  • No message buffering/queue │  │ • Content-based rule filtering│
│  • Dead-Letter Queues (DLQ)   │  │  • Sends to SQS, Lambda, SMS  │  │ • Replays historical events   │
└───────────────────────────────┘  └───────────────────────────────┘  └───────────────────────────────┘
```

---

## 🪭 2. The SNS + SQS Fan-Out Architecture Pattern

```text
                        New Order Placed Event
                                  │
                                  ▼
                        ┌──────────────────┐
                        │ Amazon SNS Topic │
                        │  "OrderCreated"  │
                        └────────┬─────────┘
            ┌────────────────────┼────────────────────┐
            ▼                    ▼                    ▼
     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
     │ SQS Queue 1  │     │ SQS Queue 2  │     │ SQS Queue 3  │
     │(Payment Svc) │     │(Inventory Svc│     │(Email Alert) │
     └──────────────┘     └──────────────┘     └──────────────┘
```

- **Advantage:** Publishers send a single notification. Each subscribing microservice receives its own isolated, buffered copy via SQS. If the inventory service goes down, payment processing continues uninterrupted!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Amazon API Gateway REST and HTTP APIs](./03-Amazon-API-Gateway-REST-and-HTTP-APIs.md) | [Index](../../../README.md) | [05 - AWS Step Functions State Machines and Orchestration →](./05-AWS-Step-Functions-State-Machines-and-Orchestration.md) |
