# 04 - Writing Molecule Test Scenarios & Verifiers

## `molecule/default/molecule.yml`
```yaml
dependency:
  name: galaxy
driver:
  name: docker
platforms:
  - name: instance-ubuntu
    image: geerlingguy/docker-ubuntu2204-ansible:latest
    pre_build_image: true
provisioner:
  name: ansible
verifier:
  name: ansible
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Molecule Framework Architecture and Drivers](./03-Molecule-Framework-Architecture-and-Drivers.md) | [Index](../../../README.md) | [05 - Idempotency Testing with Molecule →](./05-Idempotency-Testing-with-Molecule.md) |
