# 03 - Null Resource & Triggers

## 1. null_resource (Legacy Pattern)

The `null_resource` creates a resource that has no real cloud infrastructure but can execute provisioners based on trigger conditions.

```hcl
resource "null_resource" "post_deploy" {
  triggers = {
    instance_id = aws_instance.web.id
    script_hash = filemd5("scripts/configure.sh")
  }

  provisioner "local-exec" {
    command = "bash scripts/configure.sh ${aws_instance.web.private_ip}"
  }

  depends_on = [aws_instance.web]
}
```

## 2. terraform_data (Modern Replacement, >= 1.4)

```hcl
resource "terraform_data" "post_deploy" {
  triggers_replace = [
    aws_instance.web.id,
    filemd5("scripts/configure.sh"),
  ]

  provisioner "local-exec" {
    command = "bash scripts/configure.sh ${aws_instance.web.private_ip}"
  }
}
```

## 3. When Triggers Re-Execute

| Trigger Condition | What Happens |
|---|---|
| Any trigger value changes | Resource is destroyed and recreated, running provisioner again |
| `triggers = { always = timestamp() }` | Runs on EVERY apply (use sparingly) |
| Trigger values unchanged | Nothing happens (idempotent) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Connection Blocks SSH and WinRM](./02-Connection-Blocks-SSH-and-WinRM.md) | [Index](../../../README.md) | [04 - User Data and Cloud Init vs Provisioners →](./04-User-Data-and-Cloud-Init-vs-Provisioners.md) |
