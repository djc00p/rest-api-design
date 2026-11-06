# 🌐 REST API Design Patterns

[![ClawHub Skill](https://img.shields.io/badge/ClawHub-Skill-blue)](https://clawhub.ai/djc00p/rest-api-design) [![Agent Skill](https://img.shields.io/badge/Agent-Skill-blue)](#) [![REST](https://img.shields.io/badge/REST-API-00758F.svg)](https://en.wikipedia.org/wiki/Representational_state_transfer) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**REST API Design Patterns** guides the design of consistent, developer-friendly REST APIs — resource naming, HTTP semantics, and versioning.

Most API friction isn't technical. It's that one endpoint returns a bare array, another returns an envelope, a third returns `null` on error — and the consumer has to handle all three. Consistency in naming, status codes, and response shape is what makes an API pleasant to use.

---

## 🎯 When to Activate

* Designing new API endpoints
* Reviewing existing API contracts
* Adding pagination, filtering, or authentication
* Planning an API versioning strategy
* Building public or partner-facing APIs

---

## 🚀 Quick Start

1. **Use nouns** (plural, kebab-case) for resource URLs, not verbs
2. **Apply the correct HTTP method** — GET/POST/PUT/PATCH/DELETE
3. **Return appropriate status codes** — 200/201/400/401/403/404/429
4. **Use a consistent response format** — data + metadata + error structure
5. **Implement pagination** (cursor-based preferred) and filtering
6. **Require authentication** and check authorization per resource

---

## 🧭 Key Concepts

* **Resource naming** — plural nouns in kebab-case; use verbs sparingly for actions
* **HTTP semantics** — each method has idempotency/safety properties; honor them
* **Status codes** — signal intent precisely (400 for validation, 422 for semantic error)
* **Error format** — consistent structure with codes, messages, and field details
* **Versioning** — URL path versioning (`v1`, `v2`); non-breaking changes don't need a new version

---

## 🔤 Resource Naming

```text
GET    /api/v1/users
GET    /api/v1/users/:id
POST   /api/v1/users
PATCH  /api/v1/users/:id
DELETE /api/v1/users/:id
```

* **Plural nouns**, kebab-case: `/api/v1/order-items`
* **Sub-resources** for relationships: `/api/v1/users/:id/orders`
* **Verbs sparingly** — most actions are a `POST` to a resource; reserve `/actions` for things that genuinely aren't CRUD

---

## 🔢 Status Codes

| Code | Meaning |
|------|---------|
| **200** | OK — successful read or update |
| **201** | Created — with a `Location` header |
| **400** | Bad Request — validation failed |
| **401** | Unauthorized — not authenticated |
| **403** | Forbidden — authenticated but not allowed |
| **404** | Not Found |
| **409** | Conflict — state collision |
| **422** | Unprocessable — semantically invalid |
| **429** | Too Many Requests — rate limited |

The 400-vs-422 distinction matters: **400** means the request is malformed, **422** means it's well-formed but the data fails a business rule.

---

## 📦 Response Format

Consistent envelope across every endpoint:

```json
{
  "data": { "id": "usr_123", "name": "Ada" },
  "meta": { "request_id": "req_abc" },
  "error": null
}
```

**Errors** use the same envelope so consumers never parse two shapes:

```json
{
  "data": null,
  "meta": { "request_id": "req_abc" },
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "Email is required",
    "fields": { "email": "required" }
  }
}
```

---

## 📄 Pagination & Filtering

**Cursor-based is preferred** for live data — offset pagination breaks under concurrent writes and gets slower the deeper you go.

```text
GET /api/v1/orders?status=open&limit=50&cursor=eyJpZCI6...
```

---

## 🔐 Versioning

URL path versioning (`/v1`, `/v2`). A new version is only warranted by a **breaking change** — removing a field, changing a type, tightening validation. Adding an optional field is not breaking.

---

## 📚 References

* `references/rest-conventions.md` — naming, methods, status codes, and response envelopes in full
* `references/pagination.md` — cursor vs offset, filters, sorting, and total counts
* `references/auth.md` — authentication schemes, scopes, and per-resource authorization

---

**Published by [@djc00p](https://github.com/djc00p)**
