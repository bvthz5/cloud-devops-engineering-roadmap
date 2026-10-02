# 10 - Hands-On Practice Labs

## Lab 01: Multi-Provider Configuration

### Objective
Configure AWS and GCP providers in the same project.

```hcl
# providers.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

provider "google" {
  project = "my-gcp-project"
  region  = "us-central1"
}
```

```bash
terraform init
terraform providers    # List installed providers
```

---

## Lab 02: Visualize the Resource Graph

```bash
# Install graphviz
sudo apt-get install -y graphviz

# Generate graph
terraform graph | dot -Tpng > infra-graph.png
xdg-open infra-graph.png
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
