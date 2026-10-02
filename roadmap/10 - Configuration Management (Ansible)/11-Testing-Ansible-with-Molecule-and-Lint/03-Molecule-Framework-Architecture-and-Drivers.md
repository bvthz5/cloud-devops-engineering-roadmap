# 03 - Molecule Framework Architecture & Drivers

Molecule tests roles by creating ephemeral test instances (Docker, Podman, Vagrant, EC2), running playbooks, verifying system state, and destroying test infrastructure.

```bash
# Full testing matrix lifecycle
molecule test
```

Lifecycle stages:
`dependency` -> `create` -> `prepare` -> `converge` -> `idempotence` -> `verify` -> `destroy`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Static Analysis with ansible lint and yamllint](./02-Static-Analysis-with-ansible-lint-and-yamllint.md) | [Index](../../../README.md) | [04 - Writing Molecule Test Scenarios and Verifiers →](./04-Writing-Molecule-Test-Scenarios-and-Verifiers.md) |
