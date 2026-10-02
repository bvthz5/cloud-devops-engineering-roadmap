# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What is the PRIMARY purpose of `terraform state rm`?
- [ ] A) Delete the resource from the cloud provider
- [x] B) Remove a resource from Terraform's tracking without destroying it
- [ ] C) Remove the state file from the backend
- [ ] D) Unlock a stuck state lock

<details>
<summary>Explanation</summary>
terraform state rm removes a resource from state, making Terraform "forget" about it. The actual cloud resource is NOT deleted. This is useful when you want to stop managing a resource with Terraform.
</details>

---

### Question 2
Which AWS service provides state locking for the S3 backend?
- [ ] A) S3 Object Lock
- [ ] B) AWS Config
- [x] C) DynamoDB
- [ ] D) CloudFormation

<details>
<summary>Explanation</summary>
The S3 backend uses a DynamoDB table with a LockID key to implement state locking. This prevents concurrent terraform apply operations from corrupting the state.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
