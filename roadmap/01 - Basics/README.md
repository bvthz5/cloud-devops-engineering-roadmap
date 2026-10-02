# 01 — Basics & Foundations

> **Learning Methodology:**  
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

The purpose of this foundational stage is to establish complete computer hardware, operating system, binary data representations, execution runtimes, cryptography, data structures, and CLI terminal competence before progressing to Linux administration, networking, Git, Docker, Kubernetes, Terraform, and DevSecOps.

---

## 📚 Module Directory

| # | Submodule | Direct Link | Topics Covered | Status |
|---|---|---|---|---|
| **01** | **Computer Hardware & Architecture** | [01-Computer-Hardware-and-Architecture](./01-Computer-Hardware-and-Architecture/README.md) | CPU, Cache Hierarchy, Virtual Memory, NVMe, PCIe, Hypervisors, ARM vs x86 | ✅ Complete |
| **02** | **OS & Kernel Fundamentals** | [02-OS-and-Kernel-Fundamentals](./02-OS-and-Kernel-Fundamentals/README.md) | Syscalls, Processes, Threads, CFS, Inodes, Signals, cgroups v2, eBPF | ✅ Complete |
| **03** | **CLI & Terminal Basics** | [03-CLI-and-Terminal-Basics](./03-CLI-and-Terminal-Basics/README.md) | Shells, PATH, Parameter Expansions, Redirection, Pipes, Scripting, ripgrep, jq | ✅ Complete |
| **04** | **Data Formats (YAML, JSON, XML, TOML)** | [04-Data-Formats-YAML-JSON-XML-TOML](./04-Data-Formats-YAML-JSON-XML-TOML/README.md) | Serialization, JSON Schema, YAML Anchors, Norway Problem, SOPS Secrets | ✅ Complete |
| **05** | **Number Systems & Character Encoding** | [05-Number-Systems-Binary-and-Character-Encoding](./05-Number-Systems-Binary-and-Character-Encoding/README.md) | Decimal, Binary, Octal (0755), Hex, Two's Complement, ASCII, UTF-8, CRLF vs LF, Base64 | ✅ Complete |
| **06** | **Compilers, Linkers & Runtimes** | [06-Compilers-Linkers-and-Runtimes](./06-Compilers-Linkers-and-Runtimes/README.md) | Build Pipeline, Static vs Dynamic Linking, glibc vs musl, Process Memory (Heap/Stack), JIT | ✅ Complete |
| **07** | **Cryptography, PKI & Security Foundations** | [07-Cryptography-PKI-and-Security-Foundations](./07-Cryptography-PKI-and-Security-Foundations/README.md) | CIA Triad, AES-GCM, RSA vs Ed25519, SHA-256, HMAC, X.509 PKI, TLS 1.3, Argon2, /dev/urandom | ✅ Complete |
| **08** | **Data Structures, Algorithms & System Design**| [08-Data-Structures-Algorithms-and-System-Design](./08-Data-Structures-Algorithms-and-System-Design/README.md) | Big-O Notation, Hash Tables, Consistent Hashing, Queues, B-Trees, DAGs, Rate Limiting | ✅ Complete |

---

## 🧠 DevOps Foundation Mental Model

```text
                                DEVOPS FOUNDATION
                                        │
        ┌──────────────┬────────────────┼──────────────┬──────────────┐
        ▼              ▼                ▼              ▼              ▼
     Hardware         OS/Kernel      Binary/Data   Compilers/     Crypto/
   Architecture      Subsystems       Encoding      Runtimes        PKI
        │              │                │              │              │
   ┌────┴────┐    ┌────┴────┐      ┌────┴────┐    ┌────┴────┐    ┌────┴────┐
   │ CPU/RAM │    │Syscalls │      │ Decimal │    │  Build  │    │ AES/RSA │
   │ Caches  │    │Processes│      │ Binary  │    │Pipeline │    │ SHA-256 │
   │ Storage │    │Namespce │      │  UTF-8  │    │ Static/ │    │  X.509  │
   │  Buses  │    │ cgroups │      │ CRLF/LF │    │ Dynamic │    │ TLS 1.3 │
   │ Virtual │    │  eBPF   │      │ Base64  │    │glibc/mus│    │ Entropy │
   └─────────┘    └─────────┘      └─────────┘    └─────────┘    └─────────┘
        │              │                │              │              │
        └──────────────┴───────┬────────┴──────────────┴──────────────┘
                               ▼
                   CLI, Terminal & Shell Scripting
                      (Bash / Zsh / Pipelines)
                               │
                               ▼
            Data Formats & System Design Fundamentals
            (JSON / YAML / TOML / XML / Big-O / DAGs)
                               │
                               ▼
                      [ Next: 02 - Linux ]
```

---

## 🗺️ How to Study Each Module

Every module in this section follows the standardized engineering depth structure:
1. **Core Concept Files:** Granular, deep-dive technical explanations with ASCII flowcharts, hardware diagrams, and architectural analysis.
2. **Real-World Scenarios:** Production outage case studies and SRE incident post-mortems (e.g., CFS throttling, PID leaks, Norway problem, CRLF container crashes, glibc/musl incompatibilities).
3. **Troubleshooting Guides:** Diagnostic decision trees, metrics thresholds, and command one-liners (`perf`, `iostat`, `strace`, `sysctl`, `ldd`, `openssl`).
4. **Interview Q&A:** Rigorous questions covering Junior, Mid-Level, and Senior/Staff SRE depth.
5. **Hands-On Practice Labs:** Command-by-command interactive terminal exercises to run on your own machine.
6. **Multiple Choice Questions (MCQ):** Self-assessment questions with expandable technical explanations.
7. **Quick Revision Cheat Sheet:** High-density summary matrices, latency numbers, and command reference sheets.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Roadmap Root Master Index](../00-Master-Index.md) | [Roadmap Root](../00-Master-Index.md) | [02 - Linux Roadmap](../02%20-%20Linux/README.md) |
