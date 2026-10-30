# 02 - Connection Blocks: SSH & WinRM

## 1. SSH Connection

```hcl
connection {
  type        = "ssh"
  user        = "ubuntu"
  private_key = file("~/.ssh/id_rsa")
  host        = self.public_ip
  port        = 22
  timeout     = "5m"

  # Optional: bastion/jump host
  bastion_host        = "bastion.example.com"
  bastion_user        = "ec2-user"
  bastion_private_key = file("~/.ssh/bastion_key")
}
```

## 2. WinRM Connection (Windows)

```hcl
connection {
  type     = "winrm"
  user     = "Administrator"
  password = var.admin_password
  host     = self.public_ip
  port     = 5986
  https    = true
  insecure = true
  timeout  = "10m"
}
```

## 3. Security Best Practices

| Practice | Description |
|---|---|
| Use SSH keys, not passwords | Private keys are more secure than plaintext passwords |
| Use bastion/jump hosts | Don't expose SSH directly to the internet |
| Rotate keys after provisioning | One-time-use keys for provisioner access |
| Prefer cloud-native alternatives | user_data and cloud-init don't need SSH access |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Provisioner Types local exec remote exec file](./01-Provisioner-Types-local-exec-remote-exec-file.md) | [Index](../../../README.md) | [03 - Null Resource and Triggers →](./03-Null-Resource-and-Triggers.md) |
