# 01 - Provisioner Types: local-exec, remote-exec & file

## 1. Why Provisioners Exist (and Why to Avoid Them)

Provisioners are a **last resort** mechanism for running scripts on local or remote machines after resource creation. HashiCorp officially recommends avoiding them in favor of cloud-native alternatives (user_data, cloud-init, Packer).

```text
                PROVISIONER DECISION TREE
                ─────────────────────────
Can you use user_data / cloud-init?
├── YES -> Use that instead (cloud-native, no SSH needed)
└── NO  -> Can you bake it into a Packer image?
            ├── YES -> Use Packer (immutable infrastructure)
            └── NO  -> Use a provisioner (last resort)
```

## 2. The Three Built-in Provisioners

### local-exec (Runs on YOUR Machine)
```hcl
resource "aws_instance" "web" {
  ami           = "ami-abc123"
  instance_type = "t3.micro"

  provisioner "local-exec" {
    command = "echo ${self.private_ip} >> hosts.txt"
  }
}
```

### remote-exec (Runs on the REMOTE Resource via SSH/WinRM)
```hcl
resource "aws_instance" "web" {
  ami           = "ami-abc123"
  instance_type = "t3.micro"

  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl enable nginx",
    ]
  }
}
```

### file (Copies Files to the Remote Resource)
```hcl
provisioner "file" {
  source      = "scripts/setup.sh"
  destination = "/tmp/setup.sh"
}
```

## 3. Creation-Time vs Destroy-Time Provisioners

```hcl
# Runs only when resource is CREATED (default)
provisioner "local-exec" {
  command = "echo 'Resource created!'"
}

# Runs only when resource is DESTROYED
provisioner "local-exec" {
  when    = destroy
  command = "echo 'Resource destroyed!'"
}
```

## 4. Failure Behavior

```hcl
provisioner "local-exec" {
  command    = "might-fail.sh"
  on_failure = continue   # Don't mark resource as tainted on failure
  # on_failure = fail     # Default: mark resource as tainted
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Terraform-Modules-and-Reusable-Design)](../05-Terraform-Modules-and-Reusable-Design/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Connection Blocks SSH and WinRM →](./02-Connection-Blocks-SSH-and-WinRM.md) |
