# 07 - Python Automation: Real-World Production Scenarios

## Scenario 1: The OOM Crash Parsing a 15GB CloudTrail Log

### Incident Summary
A security automation script pulled daily AWS CloudTrail JSON logs into memory using `json.load(f)`. On a day with high API activity, the log grew to 14 GB. The Python script consumed 32 GB of RAM on the worker node, triggering the Linux Out-Of-Memory (OOM) killer and crashing all adjacent services.

### Root Cause
`json.load()` reads the entire file into memory as one monolithic Python dictionary.

### Resolution
Migrated to **Streaming JSON Parsing** using `ijson` or processing line-delimited JSON (JSONL) line-by-line:
```python
with open("audit_logs.jsonl", "r") as f:
    for line in f:
        event = json.loads(line)
        if event.get("eventName") == "AuthorizeSecurityGroupIngress":
            process_alert(event)
```
Memory consumption dropped from 32 GB to **25 Megabytes**, processing files of arbitrary size safely.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Structured Logging](./06-Structured-Logging-and-Defensive-Error-Handling.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
