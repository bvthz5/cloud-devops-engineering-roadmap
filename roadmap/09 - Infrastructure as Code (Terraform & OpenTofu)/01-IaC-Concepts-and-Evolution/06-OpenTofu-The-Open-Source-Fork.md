# 06 - OpenTofu: The Open-Source Terraform Fork

## 1. The License Controversy

```text
TIMELINE
────────
2014 ──► Terraform released under MPL 2.0 (open source)
2023 Aug ──► HashiCorp relicenses Terraform to BSL 1.1
             (competitors cannot offer Terraform-based managed services)
2023 Sep ──► OpenTofu manifesto launched; 140+ companies sign
2023 Sep ──► Fork created from Terraform v1.5.7 (last MPL commit)
2024 Jan ──► OpenTofu 1.6.0 GA released under Linux Foundation
2024 Mar ──► OpenTofu 1.7.0 adds client-side state encryption
2024 Jun ──► OpenTofu 1.8.0 adds early variable/local evaluation
```

## 2. BSL vs MPL — What Changed

| Aspect | MPL 2.0 (Terraform ≤ 1.5.x) | BSL 1.1 (Terraform ≥ 1.6.0) |
|---|---|---|
| **Use in production** | ✅ Unrestricted | ✅ Unrestricted |
| **Modify & redistribute** | ✅ Allowed | ⚠️ Restricted for competing products |
| **Offer as managed service** | ✅ Allowed | ❌ Prohibited (competes with HCP Terraform) |
| **Internal tooling** | ✅ Allowed | ✅ Allowed |
| **Embed in your product** | ✅ Allowed | ⚠️ Requires additional grant from HashiCorp |

## 3. OpenTofu Feature Parity & Extensions

| Feature | Terraform | OpenTofu |
|---|---|---|
| HCL language | ✅ | ✅ (fully compatible) |
| Provider ecosystem | ✅ | ✅ (same registry) |
| State file format | ✅ | ✅ (compatible) |
| **Client-side state encryption** | ❌ | ✅ (AES-GCM, AWS KMS, GCP KMS) |
| **Early variable evaluation** | ❌ | ✅ (use variables in backend config) |
| **Provider-defined functions** | ❌ | ✅ |
| Terraform Cloud integration | ✅ | ❌ (use Spacelift, env0, Scalr) |

## 4. Migration: Terraform → OpenTofu

```bash
# Step 1: Install OpenTofu
curl -fsSL https://get.opentofu.org/install-opentofu.sh | sh

# Step 2: Verify version
tofu --version

# Step 3: Initialize existing project (drop-in replacement)
cd /path/to/terraform/project
tofu init

# Step 4: Verify state compatibility
tofu plan    # Should show "No changes"

# Step 5: Update CI/CD pipelines
# Replace 'terraform' command with 'tofu'
# All flags and subcommands are identical
```

## 5. Client-Side State Encryption (OpenTofu Exclusive)

```hcl
# terraform.tf (OpenTofu only)
terraform {
  encryption {
    method "aes_gcm" "default" {
      keys = key_provider.pbkdf2.default
    }
    key_provider "pbkdf2" "default" {
      passphrase = var.state_encryption_passphrase
    }
    state {
      method = method.aes_gcm.default
    }
    plan {
      method = method.aes_gcm.default
    }
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - IaC in SDLC](./05-IaC-in-the-SDLC-and-DevOps-Pipeline.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
