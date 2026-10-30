# 01 - REST API Architecture and Idempotency

## 1. REST Architectural Constraints

REST (Representational State Transfer) is an architectural style based on:
1. **Client-Server Separation:** UI/Clients decoupled from data storage/business logic.
2. **Statelessness:** Every request must contain all context needed to process it. No session state on the server.
3. **Cacheability:** Responses must explicitly declare whether they are cacheable (`Cache-Control`).
4. **Uniform Interface:** Resources identified by clean URIs (`/api/v1/servers/123`).

---

## 2. HTTP Methods & Idempotency

An operation is **Idempotent** if making the identical request multiple times produces the exact same server side-effect as making it once.

| Method | Idempotent? | Safe? (Read-Only) | Purpose |
|---|---|---|---|
| `GET` | ✅ Yes | ✅ Yes | Retrieve resource representation |
| `HEAD` | ✅ Yes | ✅ Yes | Retrieve headers only (metadata) |
| `POST` | ❌ **No** | ❌ No | Create a new resource (non-idempotent) |
| `PUT` | ✅ Yes | ❌ No | **Replace** resource entirely |
| `PATCH` | ❌ No / Mixed | ❌ No | **Partial update** to an existing resource |
| `DELETE` | ✅ Yes | ❌ No | Remove a resource |

---

## 3. Idempotency Keys in Distributed Systems
When network failures occur during `POST /api/v1/payments`, the client cannot tell if the payment succeeded.
**Solution:** The client passes a unique `Idempotency-Key: <UUID>` header. If the server sees that UUID again, it returns the cached result without charging the card a second time!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-Golang-Basics-for-Cloud-Native)](../03-Golang-Basics-for-Cloud-Native/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - gRPC vs REST Architectural Comparison →](./02-gRPC-vs-REST-Architectural-Comparison.md) |
