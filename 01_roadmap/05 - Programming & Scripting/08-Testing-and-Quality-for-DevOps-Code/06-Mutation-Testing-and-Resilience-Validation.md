# 06 - Mutation Testing and Resilience Validation

## 1. What Is Mutation Testing?

Code coverage metrics (e.g., 90% line coverage) can be deceptively optimistic. A test suite might execute 100% of the lines in a script without asserting any meaningful output or error handling!

**Mutation testing** injects subtle bugs ("mutants") into your production code (e.g., changing `>` to `<`, flipping `True` to `False`, or removing function calls) and runs your test suite.
- If your test suite **fails**, the mutant was **killed** (good!).
- If your test suite **passes**, the mutant **survived** (indicates weak or missing assertions!).

```text
Original Code:
if disk_used_percent > 85:
    trigger_alert()

Mutant Injected by mutmut:
if disk_used_percent >= 85:   <-- Mutation 1 (boundary change)
if disk_used_percent < 85:    <-- Mutation 2 (inverted conditional)
if False:                     <-- Mutation 3 (bypassed alert)

Goal: Test suite MUST fail for each mutant!
```

---

## 2. Running `mutmut` in Python DevOps Projects

```bash
# Install mutmut
pip install mutmut

# Run mutation testing on specific automation module
mutmut run --paths-to-mutate=scripts/cloud_janitor.py

# Inspect surviving mutants
mutmut results
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Static Analysis Security Linting and CI Gates](./05-Static-Analysis-Security-Linting-and-CI-Gates.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
