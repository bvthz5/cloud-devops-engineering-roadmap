# 09 - Interview Questions & Architectural Scenarios

### Q1: Why is testing infrastructure and DevOps code inherently harder than standard application code?
**Answer**: Infrastructure code interacts with external, stateful cloud provider APIs that exhibit eventual consistency, network latency, rate limiting, and destructive actions. Spin-up and teardown times for real cloud resources (e.g., RDS databases) make testing expensive and slow, necessitating multi-tiered strategies (linters, in-memory mocks like Moto, and ephemeral container sandboxes).

### Q2: What is the risk of using `unittest.mock.MagicMock` to mock cloud SDKs like Boto3?
**Answer**: **Mock Drift**. A handcrafted mock reflects what the developer *thinks* the API returns, not what it *actually* returns. If the cloud provider schema evolves or the developer misinterprets the response structure, the unit tests pass while production execution crashes. Specialized libraries like `moto` eliminate this risk by maintaining schema-accurate emulations.

### Q3: Explain how Bats isolates test executions.
**Answer**: Bats executes each `@test` block inside a separate Bash subshell (`(...)`). Changes made to environment variables, shell options, or working directories within a test do not leak into subsequent tests.

### Q4: What is Mutation Testing and why is line coverage alone insufficient?
**Answer**: Line coverage only verifies that code was traversed, not that the assertions validate correctness. Mutation testing alters statements (e.g., flipping conditions) to verify that the test suite actually catches defects. If a mutant survives, the tests lack rigorous assertions.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
