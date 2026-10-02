# 06 - AWS SAM & Serverless Framework

Defining serverless applications using AWS SAM (`template.yaml`):
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: app.handler
      Runtime: python3.11
      Events:
        Api:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Step Functions](./05-AWS-Step-Functions-State-Machines-and-Orchestration.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
