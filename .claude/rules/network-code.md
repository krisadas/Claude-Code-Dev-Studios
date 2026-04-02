---
paths:
  - "src/networking/**"
  - "src/api/**"
---

# Network & API Code Rules

- Server is AUTHORITATIVE for all business-critical state — never trust the client
- All API requests and responses must be versioned for forward/backward compatibility
- Validate all incoming payload sizes, field types, and value ranges — reject malformed input early
- Handle timeouts, retries, and partial failures explicitly — no silent swallowing of errors
- Rate-limit all inbound endpoints to prevent abuse
- All network errors must be logged with enough context to diagnose the failure
- Authentication tokens must never be logged, even at debug level
- Use TLS for all external communication — no plaintext in production
- Document bandwidth/payload budgets for high-frequency endpoints
