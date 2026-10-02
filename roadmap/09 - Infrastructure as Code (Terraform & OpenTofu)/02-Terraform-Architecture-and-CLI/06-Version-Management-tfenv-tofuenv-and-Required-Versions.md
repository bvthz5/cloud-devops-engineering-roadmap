# 06 - Version Management: tfenv, tofuenv & Required Versions

## 1. Why Version Management Matters

Different projects may require different Terraform versions. Running the wrong version can corrupt state files or produce unexpected behavior.

## 2. tfenv (Terraform Version Manager)

```bash
# Install tfenv
git clone https://github.com/tfutils/tfenv.git ~/.tfenv
echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.bashrc

# List available versions
tfenv list-remote

# Install a specific version
tfenv install 1.5.7
tfenv install 1.7.0

# Switch versions
tfenv use 1.7.0

# Auto-switch based on .terraform-version file
echo "1.5.7" > .terraform-version
terraform --version  # → Terraform v1.5.7
```

## 3. tofuenv (OpenTofu Version Manager)

```bash
# Install tofuenv
git clone https://github.com/tofuutils/tofuenv.git ~/.tofuenv
echo 'export PATH="$HOME/.tofuenv/bin:$PATH"' >> ~/.bashrc

# Install and use
tofuenv install 1.7.0
tofuenv use 1.7.0
```

## 4. `required_version` Constraint

```hcl
terraform {
  required_version = ">= 1.5.0, < 2.0.0"
  # Prevents running with incompatible versions
}
```

```text
$ terraform plan
Error: Unsupported Terraform Core version

This configuration does not support Terraform version 1.4.6.
Required: >= 1.5.0, < 2.0.0
```

## 5. Version Strategy Matrix

| Environment | Strategy |
|---|---|
| **Development** | Latest stable version, flexible constraints |
| **CI/CD Pipeline** | Pinned exact version in `.terraform-version` |
| **Production** | Strict `required_version` + lock file |
| **Multi-team** | Organization-wide version policy document |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Terraform Graph DAG and Parallelism](./05-Terraform-Graph-DAG-and-Parallelism.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
