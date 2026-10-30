# 02 - Parallel Jobs and Work Queue Processing

## 1. Work Queue Pattern with Parallel Jobs

In an enterprise work queue architecture:
1. An external queue (Redis, RabbitMQ, AWS SQS) holds tasks to process.
2. The Job specifies `parallelism: 4` and leaves `completions` unset.
3. Each Pod loops: pulls a work item from the queue, processes it, and exits when the queue is completely empty.

```text
+-------------------+
|  AWS SQS / Redis  | ──► [Task 1, Task 2, Task 3, Task 4, Task 5 ... ]
+---------+---------+
          │
    ┌─────┴─────┬───────────┬───────────┐
    v           v           v           v
[ Worker 1 ] [ Worker 2 ] [ Worker 3 ] [ Worker 4 ]
(Pulls item, processes, loops until queue is empty, then exits 0)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Kubernetes Job Controller and Completion Guarantees](./01-Kubernetes-Job-Controller-and-Completion-Guarantees.md) | [Index](../../../README.md) | [03 - Failure Handling BackoffLimit and Pod Failure Policy →](./03-Failure-Handling-BackoffLimit-and-Pod-Failure-Policy.md) |
