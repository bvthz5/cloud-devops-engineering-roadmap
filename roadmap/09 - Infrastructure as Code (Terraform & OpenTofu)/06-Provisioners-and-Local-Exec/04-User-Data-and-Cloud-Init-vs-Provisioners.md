# 04 - User Data & Cloud-Init vs Provisioners

## 1. Why Cloud-Native Is Better

| Aspect | Provisioner | User Data / Cloud-Init |
|---|---|---|
| SSH Required | Yes | No |
| Security Group | Must allow SSH inbound | No inbound access needed |
| Execution | Terraform waits for completion | Instance boots and runs independently |
| Idempotency | Manual handling | Cloud-init is idempotent by design |
| Retry on failure | Must taint and reapply | Re-launch instance |
| Visibility | Terraform logs only | Cloud console + system logs |

## 2. AWS user_data Example

```hcl
resource "aws_instance" "web" {
  ami           = "ami-abc123"
  instance_type = "t3.micro"

  user_data = <<-EOF
    #!/bin/bash
    apt-get update
    apt-get install -y nginx
    systemctl enable nginx
    systemctl start nginx
  EOF

  user_data_replace_on_change = true
}
```

## 3. Cloud-Init with templatefile

```hcl
resource "aws_instance" "web" {
  ami           = "ami-abc123"
  instance_type = "t3.micro"

  user_data = templatefile("${path.module}/cloud-init.yaml", {
    hostname    = "web-01"
    environment = var.environment
  })
}
```

```yaml
# cloud-init.yaml
#cloud-config
hostname: ${hostname}
packages:
  - nginx
  - htop
runcmd:
  - systemctl enable nginx
  - systemctl start nginx
write_files:
  - path: /etc/environment
    content: |
      ENV=${environment}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Null Resource](./03-Null-Resource-and-Triggers.md) | [README](./README.md) | [05 - Packer Image Baking](./05-Packer-Image-Baking-vs-Runtime-Provisioning.md) |
