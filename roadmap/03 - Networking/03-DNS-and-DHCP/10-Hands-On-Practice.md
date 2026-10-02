# 10 — Hands-On Practice Labs: DNS

---

## Lab 1: Deep Root-to-Leaf DNS Tracing

```bash
# Trace delegation path for kernel.org
dig +trace kernel.org

# Observe:
# 1. Query to Root (.) servers
# 2. Delegation to .org TLD servers
# 3. Delegation to kernel.org Authoritative servers
# 4. Final Answer
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
