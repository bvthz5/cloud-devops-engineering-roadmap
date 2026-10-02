# 01 - CDKTF Architecture & Setup

## 1. What Is CDKTF?

CDK for Terraform lets you write Terraform configurations using TypeScript, Python, Java, C#, or Go instead of HCL.

```text
TypeScript Code --> cdktf synth --> HCL JSON --> terraform apply
```

## 2. Setup

```bash
npm install -g cdktf-cli
cdktf init --template=typescript --local
```

## 3. Example (TypeScript)

```typescript
import { Construct } from "constructs";
import { App, TerraformStack } from "cdktf";
import { AwsProvider } from "@cdktf/provider-aws/lib/provider";
import { Instance } from "@cdktf/provider-aws/lib/instance";

class MyStack extends TerraformStack {
  constructor(scope: Construct, id: string) {
    super(scope, id);
    new AwsProvider(this, "aws", { region: "us-east-1" });
    new Instance(this, "web", {
      ami: "ami-abc123",
      instanceType: "t3.micro",
      tags: { Name: "cdktf-web" },
    });
  }
}

const app = new App();
new MyStack(app, "my-infra");
app.synth();
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Constructs & Stacks](./02-CDKTF-Constructs-and-Stacks.md) |
