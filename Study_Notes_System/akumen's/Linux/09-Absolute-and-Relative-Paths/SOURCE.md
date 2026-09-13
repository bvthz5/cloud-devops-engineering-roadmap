# SOURCE: Absolute & Relative Paths

This document outlines the origin, foundational inputs, and structural expansions applied to create the **09-Absolute-and-Relative-Paths** topic within the **Akumen Study System**.

---

## 📌 Primary Inputs & Scope

The foundation of this topic is built upon Linux file path navigation and shell wildcard mechanisms:

1. **Path Concepts:**
   - Absolute paths (starting at `/`, deterministic, context-independent).
   - Relative paths (relative to `pwd`, context-dependent).
   - Path shortcuts (`.` for current directory, `..` for parent directory, `~` for home directory, `-` for previous working directory).
2. **Wildcard & Globbing Rules:**
   - `*` (zero or more arbitrary characters).
   - `?` (exactly one character).
   - `[]` (character set or range matching e.g., `[a-z]`, `[0-9]`).
   - `{}` (brace expansion).

---

## 🛠️ Educational Expansions & DevOps Extensions

To transform basic navigation commands into a production-grade DevOps study guide, the original material was systematically expanded to include:

- **Cron & Automation Safety:** Why absolute paths are mandatory in non-interactive system contexts (Cron jobs, Systemd services, CI/CD runners).
- **Symbolic Link Pitfalls:** How relative targets inside symlinks cause broken links when accessed from different directories.
- **Docker Mount Constraints:** Requirement of absolute host paths in Docker volume bind mounts (`-v /abs/path:/container/path`).
- **Shell Glob Expansion Mechanics:** Explaining how the shell expands wildcards before passing argument arrays to binary executables.
- **Assessment Suite:** Technical interview questions, hands-on terminal labs, multiple-choice questions, and quick revision cheat sheets.

---

## 📂 Topic File Index

- [`README.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/README.md)
- [`01-Path-Basics-and-Mental-Model.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/01-Path-Basics-and-Mental-Model.md)
- [`02-Absolute-Paths.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/02-Absolute-Paths.md)
- [`03-Relative-Paths.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/03-Relative-Paths.md)
- [`04-Dot-and-DotDot-Mechanics.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/04-Dot-and-DotDot-Mechanics.md)
- [`05-Absolute-vs-Relative-Comparison.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/05-Absolute-vs-Relative-Comparison.md)
- [`06-Wildcards-and-Shell-Globbing.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/06-Wildcards-and-Shell-Globbing.md)
- [`07-Practical-Command-Examples.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/07-Practical-Command-Examples.md)
- [`08-Real-World-Production-Scenarios.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/08-Real-World-Production-Scenarios.md)
- [`09-Troubleshooting.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/09-Troubleshooting.md)
- [`10-Interview-QA.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/10-Interview-QA.md)
- [`11-Hands-On-Practice.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/11-Hands-On-Practice.md)
- [`12-MCQ.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/12-MCQ.md)
- [`13-Quick-Revision.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/13-Quick-Revision.md)
- [`14-Related-Topics.md`](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/14-Related-Topics.md)
