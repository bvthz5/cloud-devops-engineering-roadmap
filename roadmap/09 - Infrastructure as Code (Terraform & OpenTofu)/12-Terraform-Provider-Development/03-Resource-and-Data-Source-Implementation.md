# 03 - Resource & Data Source Implementation

## 1. Resource Lifecycle

```text
terraform apply (create)  --> Create() --> API POST
terraform plan  (refresh) --> Read()   --> API GET
terraform apply (update)  --> Update() --> API PUT/PATCH
terraform destroy         --> Delete() --> API DELETE
```

## 2. Data Source (Read-Only)

```go
func (d *UserDataSource) Read(ctx context.Context, req datasource.ReadRequest, resp *datasource.ReadResponse) {
    var config UserDataModel
    req.Config.Get(ctx, &config)

    user, err := d.client.GetUser(config.ID.ValueString())
    if err != nil {
        resp.Diagnostics.AddError("Read Error", err.Error())
        return
    }

    config.Name = types.StringValue(user.Name)
    config.Email = types.StringValue(user.Email)
    resp.State.Set(ctx, config)
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Building a Custom Provider in Go](./02-Building-a-Custom-Provider-in-Go.md) | [Index](../../../README.md) | [04 - Testing and Publishing Providers →](./04-Testing-and-Publishing-Providers.md) |
