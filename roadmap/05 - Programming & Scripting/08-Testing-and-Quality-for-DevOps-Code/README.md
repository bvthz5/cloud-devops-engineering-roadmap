# 08 - Testing and Quality for DevOps Code

Treating **Infrastructure as Code (IaC)** and **Automation as Code** requires applying software engineering rigor to operational scripts. Untested Bash scripts, buggy Python automation bots, or unverified Go controllers can inadvertently wipe cloud infrastructure, compromise secret vaults, or cascade outages across Kubernetes clusters. This module covers the full automated testing and code quality lifecycle for cloud and DevOps code.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [The Testing Pyramid for Infrastructure & DevOps](./01-The-Testing-Pyramid-for-Infrastructure-and-DevOps.md) | Unit tests, Integration tests, Contract tests, End-to-End infrastructure testing, CI quality gates. |
| 02 | [Testing Bash Scripts with Bats & ShellCheck](./02-Testing-Bash-Scripts-with-Bats-and-ShellCheck.md) | ShellCheck static analysis, Bats (Bash Automated Testing System), stubbing/mocking Linux binaries. |
| 03 | [Python Testing with pytest, Moto & Mocking](./03-Python-Unit-and-Integration-Testing-pytest-and-Moto.md) | `pytest` fixtures, table parameterization, mocking AWS with `moto`, mocking HTTP with `responses`. |
| 04 | [Golang Testing & Testcontainers](./04-Golang-Testing-and-Testcontainers.md) | `testing.T`, table-driven tests, race detection (`-race`), ephemeral containers via `testcontainers-go`. |
| 05 | [Static Analysis, Security Linting & CI Gates](./05-Static-Analysis-Security-Linting-and-CI-Gates.md) | Ruff, Flake8, Black, Bandit, PSScriptAnalyzer, `golangci-lint`, pre-commit hooks integration. |
| 06 | [Mutation Testing & Resilience Validation](./06-Mutation-Testing-and-Resilience-Validation.md) | Mutation testing with `mutmut`, fault injection into automation code, testing retry and recovery logic. |
| 07 | [Real-World Scenarios & Outage Post-Mortems](./07-Real-World-Scenarios.md) | Production disasters: Root directory wipe from unquoted Bash, mock drift causing production IAM drop. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging flaky CI tests, testcontainer Docker socket permissions, Moto emulation edge cases. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 senior DevOps/SRE questions on testing automation code, test pyramid economics, and isolation. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Bats suite for backup tool; Lab 2: pytest + Moto AWS cleaner; Lab 3: Pre-commit hook setup. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 deep technical scenario MCQs with architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density cheat sheet for Bats syntax, pytest fixtures, moto decorators, and table tests. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Cloud SDKs & Infrastructure Automation](../07-Cloud-SDKs-and-Infrastructure-Automation/README.md) | [README](./README.md) | [01 - The Testing Pyramid for DevOps](./01-The-Testing-Pyramid-for-Infrastructure-and-DevOps.md) |
