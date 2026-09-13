# Antigravity Study Notes Generation Prompt & Standard Operating Procedure

This document specifies the exact instructions and standards for generating, formatting, and maintaining study modules inside `Study_Notes_System/akumen's/`.

---

## 🎯 Core Philosophy
**Understand ➔ See ➔ Practice ➔ Troubleshoot ➔ Interview ➔ Revise**

When given a broad or specific topic (e.g. *Docker*, *Linux File Permissions*, *Kubernetes Ingress*, *Git Rebase*, *Terraform State*):
1. Never provide a superficial high-level summary.
2. Deconstruct the topic into its full taxonomy, prerequisites, internals, hands-on practice, production failure scenarios, interview Q&A, and quick-revision cheat sheets.
3. Organize into the standard folder format under `Study_Notes_System/akumen's/<Category>/<Topic-Name>/`.

---

## 📁 Standard Module Structure

For each topic, create a folder containing the learning files:

```text
Study_Notes_System/akumen's/<Category>/<Topic-Name>/
├── README.md                  # High-level overview, mental models, learning goals, and prerequisites
├── 01-Basics.md               # Definitions, core concepts, essential terminology, basic commands/syntax
├── 02-Deep-Dive.md            # Deep technical details, system calls/protocols, architecture, edge cases
├── 03-How-It-Works.md         # Internal execution flow, step-by-step lifecycle, Mermaid diagrams
├── 04-Practical-Examples.md   # Copy-paste ready commands, configs, code snippets with breakdowns
├── 05-Real-World-Scenarios.md # Production architectures, industry patterns, war stories
├── 06-Troubleshooting.md      # Symptoms, error logs, root cause analysis, diagnostic commands, fixes
├── 07-Interview-QA.md         # Junior to Senior scenario-based interview questions with sample answers
├── 08-MCQ.md                  # 10+ multiple-choice questions with answer keys and in-depth explanations
├── 09-Quick-Revision.md       # Cheat sheet, one-liners, comparison tables, flashcard-style takeaways
└── 10-Related-Topics.md       # Upstream/downstream topics, tools in ecosystem, recommended next steps
```

---

## 🔄 Automatic Master Index Updates
Whenever a new module is created:
1. Register the module under the corresponding category in `Study_Notes_System/akumen's/00-Master-Index.md`.
2. Provide clickable file links so the user can easily navigate the module.
