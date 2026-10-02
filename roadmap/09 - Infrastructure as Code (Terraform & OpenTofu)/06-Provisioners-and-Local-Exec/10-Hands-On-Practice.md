# 10 - Hands-On Practice Labs

## Lab 01: local-exec to Generate Inventory

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-abc123"
  instance_type = "t3.micro"
}

resource "terraform_data" "inventory" {
  triggers_replace = [aws_instance.web[*].id]

  provisioner "local-exec" {
    command = <<-EOT
      echo "[web_servers]" > inventory.ini
      %{ for ip in aws_instance.web[*].private_ip ~}
      echo "${ip}" >> inventory.ini
      %{ endfor ~}
    EOT
  }
}
```

## Lab 02: Cloud-Init vs Provisioner Comparison

Deploy two identical instances: one with remote-exec provisioner, one with cloud-init user_data. Compare boot times and reliability.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
