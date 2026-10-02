# 07 - Real-World Scenarios

## Scenario 01: Provisioner-Induced Deployment Brittleness

### Incident
A team used `remote-exec` provisioners to install 15 packages on every EC2 instance. Network timeouts during `apt-get install` caused Terraform to taint instances, triggering cascading re-creations during retry applies.

### Resolution
1. Moved all package installation to Packer image builds
2. Terraform now launches from pre-baked AMIs with zero provisioners
3. Boot time reduced from 12 minutes to 40 seconds
4. Eliminated all SSH-related security group rules

---

## Scenario 02: local-exec for Database Migration

### Use Case
Running database migrations after RDS creation using a local-exec provisioner to call `flyway migrate`.

```hcl
resource "null_resource" "db_migration" {
  triggers = { schema_version = var.schema_version }

  provisioner "local-exec" {
    command = "flyway -url=jdbc:postgresql://${aws_db_instance.main.endpoint}/mydb migrate"
    environment = {
      FLYWAY_USER     = var.db_admin_user
      FLYWAY_PASSWORD = var.db_admin_password
    }
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - External Data Source and Custom Scripts](./06-External-Data-Source-and-Custom-Scripts.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
