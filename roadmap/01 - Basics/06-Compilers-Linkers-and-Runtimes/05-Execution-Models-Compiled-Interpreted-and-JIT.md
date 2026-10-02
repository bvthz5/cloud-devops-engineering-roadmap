# 05 — Execution Models: Compiled, Interpreted, and JIT

Different programming languages execute code through different runtime architectures, directly affecting container cold start latency, CPU efficiency, and memory consumption.

---

## 1. The Three Primary Execution Paradigms

```text
1. Ahead-of-Time (AOT) Compiled (C, C++, Go, Rust)
   Source Code  ──[ Compiler ]──> Machine Code (Native CPU Instructions)
   Direct execution on hardware with zero VM overhead.

2. Pure Interpreted (Bash, Python, Ruby, PHP)
   Source Code  ──[ Interpreter ]──> Read & Execute line-by-line in software
   Higher CPU overhead; no compile step required.

3. Bytecode + Just-In-Time (JIT) (Java JVM, C# .NET, Node.js V8)
   Source Code  ──[ Compiler ]──> Intermediate Bytecode (.class)
                                         │
                                         ▼
                               [ Virtual Machine / JIT ]
                               Compiles hot paths to native machine code at runtime!
```

---

## 2. Cloud & Serverless Comparison

| Metric | AOT (Go, Rust) | JIT (Java, .NET) | Interpreted (Python, Node.js) |
| :--- | :--- | :--- | :--- |
| **Cold Start Latency**| **Microseconds (< 10ms)** | Slow (500ms - 3s JVM boot) | Fast (50ms - 200ms) |
| **Base Memory Footprint**| **Tiny (10 - 30 MB)** | Large (150 - 500 MB JVM overhead)| Moderate (30 - 80 MB) |
| **Peak Throughput** | Maximum | Extremely High (JIT optimizations)| Moderate to Low |
| **Best Cloud Use Case** | Microservices, CLI, AWS Lambda | Heavy Enterprise Data Processing | Glue scripts, Web APIs, Automation |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Process Memory Layout](./04-Process-Memory-Layout-Heap-Stack-and-Segments.md) | [README](./README.md) | [06 - Dynamic Linker](./06-Dynamic-Linker-and-Symbol-Resolution.md) |
