# 06 - History Limits, Cleanup, and Deadlines

## 1. Garbage Collection with `ttlSecondsAfterFinished`

Completed Job pods linger in `Completed` status so engineers can inspect logs. Without cleanup, thousands of old jobs accumulate, causing API server slowdown:

```yaml
spec:
  ttlSecondsAfterFinished: 600   # Automatically deletes the Job and its Pods 10m after finish!
```

---

## 2. Preventing Runaway Jobs with `activeDeadlineSeconds`

If a batch job hangs on a deadlocked TCP connection or infinite loop, it will consume cluster resources indefinitely:

```yaml
spec:
  activeDeadlineSeconds: 3600    # Hard termination after 1 hour! Kills all pods immediately.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Concurrency Policies Allow Forbid and Replace](./05-Concurrency-Policies-Allow-Forbid-and-Replace.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
