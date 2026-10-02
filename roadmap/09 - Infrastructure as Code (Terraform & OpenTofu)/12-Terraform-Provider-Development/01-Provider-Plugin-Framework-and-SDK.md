# 01 - Provider Plugin Framework & SDK

## 1. SDK v2 vs Plugin Framework

| Aspect | SDKv2 (Legacy) | Plugin Framework (New) |
|---|---|---|
| Status | Maintenance mode | Active development |
| Language | Go | Go |
| Protocol | v5 | v5 + v6 |
| Schema | Schema map | Typed schema with attributes |
| Testing | Resource-level acceptance tests | Unit + acceptance tests |
| Recommended for | Existing providers | New providers |

## 2. Provider Architecture

```text
Terraform Core <--gRPC--> Provider Binary
                              |
                              +-- Provider Schema (configuration)
                              +-- Resource Implementations (CRUD)
                              +-- Data Source Implementations (Read)
                              +-- API Client (HTTP calls to target API)
```

## 3. Plugin Framework Example

```go
package provider

import (
    "github.com/hashicorp/terraform-plugin-framework/provider"
    "github.com/hashicorp/terraform-plugin-framework/provider/schema"
)

type MyProvider struct{}

func (p *MyProvider) Schema(_ context.Context, _ provider.SchemaRequest, resp *provider.SchemaResponse) {
    resp.Schema = schema.Schema{
        Attributes: map[string]schema.Attribute{
            "api_url": schema.StringAttribute{
                Required:    true,
                Description: "Base URL for the API",
            },
            "api_token": schema.StringAttribute{
                Required:    true,
                Sensitive:   true,
                Description: "Authentication token",
            },
        },
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (11-Terraform-Cloud-and-Enterprise)](../11-Terraform-Cloud-and-Enterprise/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Building a Custom Provider in Go →](./02-Building-a-Custom-Provider-in-Go.md) |
