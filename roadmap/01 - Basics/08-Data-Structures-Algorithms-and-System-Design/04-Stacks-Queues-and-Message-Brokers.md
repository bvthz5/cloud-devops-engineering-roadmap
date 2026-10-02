# 04 — Stacks, Queues, and Message Brokers

Queues and stacks govern inter-process communication, asynchronous task decoupling, and backpressure management in microservices.

---

## 1. LIFO vs FIFO

- **Stack (LIFO - Last In, First Out):** Push and pop from the top.
  - Used in CPU call stacks, undo buffers, memory allocators.
- **Queue (FIFO - First In, First Out):** Enqueue at tail, dequeue at head.
  - Used in print queues, CPU scheduler run queues, web server connection backlogs, and message brokers.

---

## 2. Distributed Message Queues (Kafka & RabbitMQ)

```text
[ Producers ] ───( Publish Message )───> [ Message Queue / Broker ] ───( Consume )───> [ Consumers ]
                                          ├── Worker Queue
                                          ├── Dead-Letter Queue (DLQ)
                                          └── Persistent Storage
```

### Critical SRE Queue Concepts:
- **Backpressure:** When consumers process slower than producers publish, queue depth grows. Without flow control, memory exhausts.
- **Dead-Letter Queue (DLQ):** Unprocessable messages (poison pills) are isolated into a DLQ after $N$ failed retries to prevent blocking healthy messages.
- **At-least-once vs Exactly-once Delivery:** Network retries mean consumers must be **idempotent** (processing the same message twice causes no side effects).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Consistent Hashing in Distributed Systems](./03-Consistent-Hashing-in-Distributed-Systems.md) | [Index](../../../README.md) | [05 - Trees and Graph Data Structures →](./05-Trees-and-Graph-Data-Structures.md) |
