# Module 02: Python for DevOps & Infrastructure Automation

Welcome to **Module 02: Python for DevOps and Automation**. When automation scripts exceed 100 lines, require complex data structures, or interact with cloud REST APIs, Bash becomes unmaintainable. Python is the premier scripting language for SRE, cloud automation, and tooling.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Leverage Python data structures (lists, dicts, sets, comprehensions) for fast log and data parsing.
2. Execute system binaries and capture streams securely using the **`subprocess`** module (`check_output`, `run`).
3. Build production CLI tools with flags, subcommands, and help text using **`argparse`** and **`click`**.
4. Interact with external web services and REST APIs with connection pooling and retries using **`requests`** and **`httpx`**.
5. Parse, transform, and validate **JSON**, **YAML**, and **TOML** configurations.
6. Manage dependencies cleanly using virtual environments (`venv`, `poetry`, `uv`) and write structured JSON logs.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Python DevOps Foundations & Environment](./01-Python-DevOps-Foundations-and-Environment.md) | Virtual environments (`venv`, `uv`), packaging, typing hints, modern Python 3.12+ |
| 02 | [Subprocess Execution & System Management](./02-Subprocess-Execution-and-System-Management.md) | `subprocess.run`, capturing stdout/stderr, pipe chaining, timeout handling |
| 03 | [Building CLI Tools: argparse & click](./03-Building-Production-CLI-Tools-argparse-and-click.md) | Argument parsing, flags, subcommands, beautiful terminal outputs with Rich |
| 04 | [REST API Automation: requests & httpx](./04-REST-API-Automation-requests-and-httpx.md) | Connection pooling, bearer token auth, retry adapters with exponential backoff |
| 05 | [Parsing Configs: JSON, YAML & TOML](./05-Config-Parsing-JSON-YAML-and-TOML.md) | `json`, `ruamel.yaml`, `tomllib`, safe loading, schema validation with Pydantic |
| 06 | [Structured Logging & Error Handling](./06-Structured-Logging-and-Defensive-Error-Handling.md) | JSON logging for Datadog/Elastic, custom exceptions, context managers |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Script hangs indefinitely waiting on dead socket, memory leak parsing 10GB log |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Debugging subprocess deadlock, SSL cert verification failures, dependency drift |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps Python interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Writing an automated GitHub API repository auditor with rate-limit retries |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Subprocess reference, requests session template, logging configuration |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Bash Scripting](../01-Bash-Scripting-for-DevOps/README.md) | [README](./README.md) | [01 - Python Foundations](./01-Python-DevOps-Foundations-and-Environment.md) |
