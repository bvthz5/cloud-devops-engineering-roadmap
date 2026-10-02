# 06 - External Data Source & Custom Scripts

## 1. External Data Source

The `external` data source calls an external script and reads its JSON output as data.

```hcl
data "external" "next_cidr" {
  program = ["python3", "${path.module}/scripts/next_cidr.py"]
  query = {
    vpc_id = aws_vpc.main.id
  }
}

# Access the result
resource "aws_subnet" "next" {
  cidr_block = data.external.next_cidr.result["cidr"]
  vpc_id     = aws_vpc.main.id
}
```

### Script Requirements
- Read JSON from stdin (`query` object)
- Write JSON to stdout (`result` object)
- Must be deterministic (same input = same output)

```python
#!/usr/bin/env python3
import json, sys
input_data = json.load(sys.stdin)
# Process...
json.dump({"cidr": "10.0.5.0/24"}, sys.stdout)
```

## 2. Integration with Ansible

```hcl
resource "null_resource" "ansible_playbook" {
  triggers = {
    instance_ids = join(",", aws_instance.web[*].id)
  }

  provisioner "local-exec" {
    command = <<-EOT
      ansible-playbook -i '${join(",", aws_instance.web[*].private_ip)},' \
        --private-key ~/.ssh/deploy_key \
        playbooks/configure.yml
    EOT
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Packer Image Baking](./05-Packer-Image-Baking-vs-Runtime-Provisioning.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
