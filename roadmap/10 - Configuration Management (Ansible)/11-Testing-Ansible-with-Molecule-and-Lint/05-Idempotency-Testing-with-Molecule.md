# 05 - Idempotency Testing with Molecule

Molecule runs the `converge` playbook a second time. If any task returns `changed=true` on the second run, Molecule fails the idempotency test step.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Writing Molecule Scenarios](./04-Writing-Molecule-Test-Scenarios-and-Verifiers.md) | [README](./README.md) | [06 - Cross-OS Testing](./06-Multi-Instance-and-Cross-OS-Testing-Scenarios.md) |
