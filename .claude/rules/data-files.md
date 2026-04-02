---
paths:
  - "assets/data/**"
  - "config/**"
---

# Data & Config File Rules

- All JSON/YAML files must be valid and schema-validated — broken config must not reach production
- File naming: lowercase with underscores only, following `[domain]_[name].[ext]` pattern
- Every data file must have a documented schema (JSON Schema, YAML schema, or documented in the corresponding feature doc)
- Numeric values must include comments or companion docs explaining their meaning and safe ranges
- Use consistent key naming: camelCase for JSON keys, snake_case for YAML keys
- No orphaned entries — every entry must be referenced by code or another config file
- Version data files when making breaking schema changes
- Include sensible defaults for all optional fields
- Secrets must NEVER be stored in data files — use environment variables or a secrets manager

## Examples

**Correct** naming and structure (`users_roles.json`):

```json
{
  "admin": {
    "displayName": "Administrator",
    "permissions": ["read", "write", "delete"],
    "sessionTimeoutMinutes": 60
  },
  "viewer": {
    "displayName": "Viewer",
    "permissions": ["read"],
    "sessionTimeoutMinutes": 1440
  }
}
```

**Incorrect** (`RoleData.json`):

```json
{
  "Admin": { "perms": ["read", "write", "delete"] }
}
```

Violations: uppercase filename, uppercase key, no `[domain]_[name]` pattern, missing required fields.
