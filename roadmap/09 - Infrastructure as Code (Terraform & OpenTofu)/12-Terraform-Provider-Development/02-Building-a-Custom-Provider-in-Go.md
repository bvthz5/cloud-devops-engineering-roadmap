# 02 - Building a Custom Provider in Go

## 1. Project Structure

```text
terraform-provider-myapi/
+-- main.go                  # Entry point
+-- internal/
|   +-- provider/
|   |   +-- provider.go      # Provider implementation
|   +-- resources/
|   |   +-- user_resource.go # User resource CRUD
|   +-- datasources/
|       +-- user_data.go     # User data source (read-only)
+-- go.mod
+-- go.sum
+-- GNUmakefile
```

## 2. Provider Registration

```go
func main() {
    opts := providerserver.ServeOpts{
        Address: "registry.terraform.io/myorg/myapi",
    }
    providerserver.Serve(context.Background(), NewProvider, opts)
}
```

## 3. Resource CRUD Pattern

```go
func (r *UserResource) Create(ctx context.Context, req resource.CreateRequest, resp *resource.CreateResponse) {
    var plan UserModel
    diags := req.Plan.Get(ctx, &plan)
    resp.Diagnostics.Append(diags...)
    if resp.Diagnostics.HasError() { return }

    // Call API
    user, err := r.client.CreateUser(plan.Name.ValueString(), plan.Email.ValueString())
    if err != nil {
        resp.Diagnostics.AddError("Create Error", err.Error())
        return
    }

    // Map API response to state
    plan.ID = types.StringValue(user.ID)
    diags = resp.State.Set(ctx, plan)
    resp.Diagnostics.Append(diags...)
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Plugin Framework](./01-Provider-Plugin-Framework-and-SDK.md) | [README](./README.md) | [03 - Resource Implementation](./03-Resource-and-Data-Source-Implementation.md) |
