# 01 — Basics & Foundations

> **Learning Methodology:**  
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

The purpose of this foundational stage is to establish complete computer hardware, operating system, CLI terminal, and data structure competence before progressing to Linux administration, networking, Git, Docker, Kubernetes, Terraform, and DevSecOps.

---

## 📚 Module Directory

| # | Submodule | Direct Link | Topics Covered | Status |
|---|---|---|---|---|
| **01** | **Computer Hardware & Architecture** | [01-Computer-Hardware-and-Architecture](./01-Computer-Hardware-and-Architecture/README.md) | 74 Topics (CPU, Cache, Virtual Memory, NVMe, PCIe, Hypervisors, ARM vs x86) | Complete |
| **02** | **OS & Kernel Fundamentals** | [02-OS-and-Kernel-Fundamentals](./02-OS-and-Kernel-Fundamentals/README.md) | 90 Topics + Modern (Syscalls, Processes, Threads, CFS, Inodes, Signals, cgroups v2, eBPF) | Complete |
| **03** | **CLI & Terminal Basics** | [03-CLI-and-Terminal-Basics](./03-CLI-and-Terminal-Basics/README.md) | 90 Topics + Tools (Shells, PATH, Parameter Expansions, Redirection, Pipes, Scripting, ripgrep, jq) | Complete |
| **04** | **Data Formats (YAML, JSON, XML, TOML)** | [04-Data-Formats-YAML-JSON-XML-TOML](./04-Data-Formats-YAML-JSON-XML-TOML/README.md) | 90 Topics (Serialization, JSON Schema, YAML Anchors, Norway Problem, SOPS Secrets) | Complete |

---

## 🧠 DevOps Foundation Mental Model

```text
                               DEVOPS FOUNDATION
                                       │
        ┌──────────────────────────────┼──────────────────────────────┐
        ▼                              ▼                              ▼
     Hardware                          OS                            Data
   Architecture                    Kernel Core                     Formats
        │                              │                              │
   ┌────┴────┐                    ┌────┴────┐                    ┌────┴────┐
   │ CPU/RAM │                    │Syscalls │                    │  YAML   │
   │ Caches  │                    │Processes│                    │  JSON   │
   │ Storage │                    │Namespce │                    │  TOML   │
   │  Buses  │                    │ cgroups │                    │   XML   │
   └─────────┘                    └─────────┘                    └─────────┘
        │                              │                              │
        └──────────────────────┬───────┴──────────────────────────────┘
                               ▼
                         CLI & Terminal
                   (Bash / Zsh / Pipelines)
                               │
                               ▼
                   [ Next: 02 - Linux ]
```

---

## 🗺️ How to Study Each Module

Every module in this section follows the standardized engineering depth structure:
1. **Core Concept Files:** Granular, deep-dive technical explanations with ASCII flowcharts, hardware diagrams, and architectural analysis.
2. **Real-World Scenarios:** Production outage case studies and SRE incident post-mortems (e.g., CFS throttling, PID leaks, Norway problem).
3. **Troubleshooting Guides:** Diagnostic decision trees, metrics thresholds, and command one-liners (`perf`, `iostat`, `strace`, `sysctl`).
4. **Interview Q&A:** Rigorous questions covering Junior, Mid-Level, and Senior/Staff SRE depth.
5. **Hands-On Practice Labs:** Command-by-command interactive terminal exercises to run on your own machine.
6. **Multiple Choice Questions (MCQ):** Self-assessment questions with expandable technical explanations.
7. **Quick Revision Cheat Sheet:** High-density summary matrices, latency numbers, and command reference sheets.
