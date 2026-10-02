# 11 - Multiple-Choice Assessment (MCQ)

### 1. In AWS Boto3, what retry mode dynamically adjusts request rates based on service throttling?
- [ ] A) `standard`
- [ ] B) `legacy`
- [x] C) `adaptive`
- [ ] D) `exponential`

<details>
<summary><b>Explanation</b></summary>
The <code>adaptive</code> retry mode in Boto3 includes client-side rate limiting that dynamically throttles outgoing requests when encountering <code>ThrottlingException</code> responses.
</details>

---

### 2. In Azure Python SDK, which object is returned by long-running operations like VM provisioning?
- [ ] A) `TaskPromise`
- [x] B) `LROPoller`
- [ ] C) `AsyncOperation`
- [ ] D) `FutureResult`

<details>
<summary><b>Explanation</b></summary>
The Azure SDK uses <code>LROPoller</code> (Long Running Operation Poller) to provide status inspection and blocking completion waits for asynchronous resource state operations.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
