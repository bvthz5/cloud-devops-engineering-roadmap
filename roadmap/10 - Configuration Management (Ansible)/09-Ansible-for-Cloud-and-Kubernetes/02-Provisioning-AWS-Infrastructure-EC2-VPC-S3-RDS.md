# 02 - Provisioning AWS Infrastructure

```yaml
- name: Create EC2 Instance
  amazon.aws.ec2_instance:
    name: "web-server-01"
    key_name: "my-key"
    instance_type: "t3.micro"
    image_id: "ami-0c55b159cbfafe1f0"
    region: "us-east-1"
    vpc_subnet_id: "subnet-123456"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Cloud Automation with Ansible AWS Azure GCP](./01-Cloud-Automation-with-Ansible-AWS-Azure-GCP.md) | [Index](../../../README.md) | [03 - Kubernetes Automation with kubernetes core Collection →](./03-Kubernetes-Automation-with-kubernetes-core-Collection.md) |
