# 05 - Programming & Scripting for Cloud, DevOps & Platform Engineering

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

A modern Cloud, DevOps, or Site Reliability Engineer must possess robust programming and scripting capabilities. Beyond simple ad-hoc one-liners, platform engineering mandates building **production-grade automation, cloud SDK workflows, custom Kubernetes controllers, and self-healing systems** with comprehensive testing and enterprise CI quality gates.

---

## 📌 Complete Curriculum Modules

| # | Module | Core Focus & Engineering Scope | Status |
|---|---|---|:---:|
| 01 | [**01. Bash Scripting for DevOps**](./01-Bash-Scripting-for-DevOps/README.md) | Shell architecture, POSIX standards, subshells, process substitution, robust options (`set -euo pipefail`), traps, and Linux signals. | ✅ Complete |
| 02 | [**02. Python for DevOps & Automation**](./02-Python-for-DevOps-and-Automation/README.md) | Python runtime internals, memory management, `asyncio`, type hints, structured logging, packaging (`pyproject.toml`), and Click CLI frameworks. | ✅ Complete |
| 03 | [**03. Golang Basics for Cloud-Native**](./03-Golang-Basics-for-Cloud-Native/README.md) | Go memory model, goroutines, CSP concurrency with channels, context cancellation, interfaces, and microservice compilation. | ✅ Complete |
| 04 | [**04. APIs: REST, gRPC & Webhooks**](./04-APIs-REST-gRPC-and-Webhooks/README.md) | REST architecture, idempotency, HTTP/2 & Protobuf gRPC streaming, webhook HMAC-SHA256 signatures, OAuth2/mTLS, and adaptive rate limiting. | ✅ Complete |
| 05 | [**05. Automation Script Templates**](./05-Automation-Script-Templates/README.md) | Production templates: Disk watchdog log cleaner, cloud snapshot retention, Kube pod remediator, SSL cert scanner, and PagerDuty dispatcher. | ✅ Complete |
| 06 | [**06. PowerShell Core for Cloud & DevOps**](./06-PowerShell-Core-for-Cloud-and-DevOps/README.md) | Cross-platform `pwsh`, .NET object pipeline, advanced functions (`[CmdletBinding()]`), Az module, AWS Tools, and stream redirection. | ✅ Complete |
| 07 | [**07. Cloud SDKs & Infrastructure Automation**](./07-Cloud-SDKs-and-Infrastructure-Automation/README.md) | AWS Boto3 (client vs resource, paginators, waiters), Azure SDK (`DefaultAzureCredential`, `LROPoller`), GCP ADC, and cloud cost janitors. | ✅ Complete |
| 08 | [**08. Testing & Quality for DevOps Code**](./08-Testing-and-Quality-for-DevOps-Code/README.md) | Testing pyramid for IaC, ShellCheck, Bats for Bash, `pytest` + `moto` for AWS, Go testing with `testcontainers`, mutation testing, and CI gates. | ✅ Complete |
| 09 | [**09. Kubernetes Client-Go & Custom Controllers**](./09-Kubernetes-Client-Go-and-Custom-Controllers/README.md) | Kubernetes API machinery, `client-go` Informers/Listers/Workqueue, CRDs, Reconcile loop, Kubebuilder/Operator SDK, and Cobra/Viper CLIs. | ✅ Complete |

---

## 🛠️ Module Structure Standard

Every module in this domain strictly adheres to the enterprise 13-file roadmap standard:
1. `README.md` — Architectural overview & module syllabus
2. `01-` to `06-` — Deep technical guides with diagrams, code blocks, and internal mechanisms
3. `07-Real-World-Scenarios.md` — Real production post-mortems and multi-stage failure analyses
4. `08-Troubleshooting.md` — Diagnostic decision trees and debugging runbooks
5. `09-Interview-QA.md` — 10 Senior SRE/DevOps technical interview scenarios
6. `10-Hands-On-Practice.md` — Hands-on implementation labs with production blueprints
7. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible explanations
8. `12-Quick-Revision.md` — High-density cheat sheets and command reference tables

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Git & Version Control](../04%20-%20Git%20&%20Version%20Control/README.md) | [Master Index](../00-Master-Index.md) | [06 - Web Servers & Reverse Proxies](../06%20-%20Web%20Servers%20&%20Reverse%20Proxies/README.md) |
