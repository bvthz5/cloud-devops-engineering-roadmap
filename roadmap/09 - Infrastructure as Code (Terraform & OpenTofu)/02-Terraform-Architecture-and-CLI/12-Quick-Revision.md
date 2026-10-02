# 12 - Quick-Revision & Enterprise Cheat Sheet

## Architecture Quick Reference

| Component | Role |
|---|---|
| **Core Binary** | Parses HCL, builds DAG, manages state |
| **Provider Plugin** | Translates resource CRUD into cloud API calls |
| **gRPC Protocol** | Communication channel between Core and Providers |
| **DAG Engine** | Determines execution order and parallelism |
| **State Manager** | Tracks real-world resource IDs and attributes |

## Essential CLI Commands

| Command | Purpose |
|---|---|
| `terraform init` | Initialize project, download providers |
| `terraform init -upgrade` | Update providers within constraints |
| `terraform init -migrate-state` | Migrate state to new backend |
| `terraform providers` | List required providers |
| `terraform graph` | Output resource dependency graph (DOT format) |
| `terraform force-unlock <ID>` | Release stuck state lock |

## Key Environment Variables

| Variable | Purpose |
|---|---|
| `TF_LOG=DEBUG` | Enable debug logging |
| `TF_PLUGIN_CACHE_DIR` | Cache provider binaries |
| `TF_CLI_ARGS_plan` | Default args for `plan` subcommand |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 03 - HCL Syntax](../03-HCL-Syntax-Variables-and-Outputs/README.md) |
