# 01 - Basics & Foundations

> **Learning Methodology:** Understand ➔ See ➔ Practice ➔ Troubleshoot ➔ Interview ➔ Revise

The purpose of this foundational stage is to establish complete computer hardware, operating system, CLI terminal, and data structure competence before progressing to Linux administration, networking, Git, Docker, Kubernetes, Terraform, and DevSecOps.

---

## 📌 Module Directory

| # | Submodule | Direct Link | Status |
|---|---|---|---|
| **01** | Computer Hardware & Architecture | [01-Computer-Hardware-and-Architecture](01-Computer-Hardware-and-Architecture/README.md) | ✅ Complete |
| **02** | OS & Kernel Fundamentals | [02-OS-and-Kernel-Fundamentals](02-OS-and-Kernel-Fundamentals/README.md) | ✅ Complete |
| **03** | CLI & Terminal Basics | [03-CLI-and-Terminal-Basics](03-CLI-and-Terminal-Basics/README.md) | ✅ Complete |
| **04** | Data Formats (YAML, JSON, XML, TOML) | [04-Data-Formats-YAML-JSON-XML-TOML](04-Data-Formats-YAML-JSON-XML-TOML/README.md) | ✅ Complete |
| **05** | Foundational Exercises & Troubleshooting | [05-Foundational-Exercises-and-Troubleshooting](05-Foundational-Exercises-and-Troubleshooting/README.md) | ✅ Complete |

---

## 🗺️ DevOps Foundation Mental Model

```text
                               DEVOPS FOUNDATION
                                       │
        ┌──────────────────────────────┼──────────────────────────────┐
        │                              │                              │
     Hardware                          OS                            Data
        │                              │                           Formats
        │                              │                              │
   ┌────┼────┐                   ┌─────┼─────┐                 ┌─────┼─────┐
   │    │    │                   │     │     │                 │     │     │
 CPU  RAM Storage              Kernel Process Shell            YAML JSON  XML/TOML
   │    │    │                   │     │     │
   └────┴────┴───────────────────┴─────┴─────┘
                           │
                           ↓
                         CLI & Terminal
                           │
                           ↓
                    Layered Troubleshooting
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
      Linux               Git               Docker
        ↓                  ↓                  ↓
   Networking            CI/CD             Kubernetes
        ↓                  ↓                  ↓
      Cloud            Terraform          Observability
```
