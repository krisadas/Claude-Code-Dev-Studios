---
paths:
  - "tests/**"
---

# Test Standards

- Test naming: `test_[module]_[scenario]_[expected_result]` pattern
- Every test must have a clear arrange/act/assert structure
- Unit tests must not depend on external state (filesystem, network, database)
- Integration tests must clean up after themselves
- Performance tests must specify acceptable thresholds and fail if exceeded
- Test data must be defined in the test or in dedicated fixtures, never shared mutable state
- Mock external dependencies — tests must be fast and deterministic
- Every bug fix must have a regression test that would have caught the original bug

## Examples

**Correct** (proper naming + Arrange/Act/Assert):

```typescript
test('userService_createUser_withDuplicateEmail_throwsConflictError', async () => {
  // Arrange
  const repo = new FakeUserRepository({ existing: [{ email: 'a@b.com' }] })
  const service = new UserService(repo)

  // Act
  const result = service.createUser({ email: 'a@b.com', name: 'Alice' })

  // Assert
  await expect(result).rejects.toThrow(ConflictError)
})
```

**Incorrect**:

```typescript
test('user test', async () => {   // VIOLATION: no descriptive name
  const s = new UserService(realDb)  // VIOLATION: uses real DB
  await s.createUser({ email: 'a@b.com', name: 'Alice' })
  // VIOLATION: no assertion
})
```
