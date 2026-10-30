# 10 — Hands-On Practice Labs: Data Structures & Algorithms

---

## Lab 1: Benchmarking List vs Set Lookup ($O(N)$ vs $O(1)$)

```python
# Run this Python script to feel the real-world difference between O(N) and O(1)
import time

size = 100_000
test_list = list(range(size))
test_set = set(range(size))

target = 99_999

# Benchmark List O(N)
start = time.perf_counter()
for _ in range(100):
    _ = target in test_list
print(f"List O(N) Time: {(time.perf_counter() - start):.4f} seconds")

# Benchmark Set O(1)
start = time.perf_counter()
for _ in range(100):
    _ = target in test_set
print(f"Set O(1) Time:  {(time.perf_counter() - start):.6f} seconds")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
