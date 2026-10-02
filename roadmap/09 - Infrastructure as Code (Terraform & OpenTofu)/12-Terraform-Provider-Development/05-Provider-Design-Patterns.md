# 05 - Provider Design Patterns

## 1. Key Patterns

| Pattern | Description |
|---|---|
| **Retry with backoff** | Retry API calls on transient errors (429, 503) |
| **Pagination** | Handle paginated API responses for list operations |
| **Computed attributes** | Mark server-generated fields as Computed |
| **Import support** | Implement ImportState for existing resources |
| **Timeouts** | Configurable create/update/delete timeouts |

## 2. Import Support

```go
func (r *UserResource) ImportState(ctx context.Context, req resource.ImportStateRequest, resp *resource.ImportStateResponse) {
    resource.ImportStatePassthroughID(ctx, path.Root("id"), req, resp)
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Testing and Publishing Providers](./04-Testing-and-Publishing-Providers.md) | [Index](../../../README.md) | [06 - Community Providers and Contribution →](./06-Community-Providers-and-Contribution.md) |
