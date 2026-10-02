# 04 - Testing & Publishing Providers

## 1. Acceptance Testing

```go
func TestAccUserResource(t *testing.T) {
    resource.Test(t, resource.TestCase{
        ProtoV6ProviderFactories: testAccProtoV6ProviderFactories,
        Steps: []resource.TestStep{
            {
                Config: `resource "myapi_user" "test" { name = "Test User" email = "test@example.com" }`,
                Check: resource.ComposeAggregateTestCheckFunc(
                    resource.TestCheckResourceAttr("myapi_user.test", "name", "Test User"),
                    resource.TestCheckResourceAttrSet("myapi_user.test", "id"),
                ),
            },
        },
    })
}
```

## 2. Publishing to Registry

- Repository must be named `terraform-provider-<NAME>`
- Must have GoReleaser config for cross-compilation
- Sign releases with GPG key registered on Terraform Registry
- Tag releases with semantic versioning: `v1.0.0`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Resource Implementation](./03-Resource-and-Data-Source-Implementation.md) | [README](./README.md) | [05 - Design Patterns](./05-Provider-Design-Patterns.md) |
