---
paths:
  - "src/ai/**"
---

# AI / ML Code Rules

- All model parameters and thresholds must be configurable from data/config files, never hardcoded
- AI features must degrade gracefully when the model or service is unavailable
- Log all AI inputs and outputs at debug level to aid in debugging and auditing
- Never pass sensitive user data (PII, credentials) to external AI APIs without explicit consent and sanitization
- All AI-generated content must be validated before being persisted or displayed to users
- Rate-limit AI API calls — implement backoff and circuit-breaker patterns
- Cache AI responses where appropriate to reduce latency and cost
- Document the expected input/output contract for every AI feature
- AI decision logic must be auditable: store enough context to replay or explain decisions
