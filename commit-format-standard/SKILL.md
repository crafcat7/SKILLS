---
name: commit-format-standard
description: 规范 Git commit 信息的格式。当用户要求提交代码、创建 commit、撰写 commit message，或任何涉及版本控制提交操作的场景，都应使用此 skill。此 skill 确保 commit 信息包含清晰的类型作用域描述和详细的 Summary。
---

# Commit 格式规范

每次 commit 信息必须严格遵循以下结构。

## 格式模板

```
[<type>] <scope>: <description>

Summary:
  <detailed explanation, including purpose, solution, and impact>

Changes:
  1. <change point 1>
  2. <change point 2>
  3. <change point 3>
```

## 格式说明

### 第一行格式：`[<type>] <scope>: <description>`

| Component | Rule |
|-----------|------|
| `type` | Lowercase: feat, fix, refactor, test, docs, chore, perf, etc. |
| `scope` | Module or component name (optional) |
| `description` | Verb first, brief change description, within 50 characters |

### Summary

- Indent with 2 spaces
- Use English
- Explain: what, why, and impact
- Usually 1-3 sentences

### Changes

- Use numbered list (1. 2. 3.)
- Each item should be a specific, independent change
- Keep concise, one sentence per item
- Order by importance or logical sequence

## Examples

**Example 1: Feature**
```
[feat] user-auth: add JWT token refresh mechanism

Summary:
  Implement token refresh endpoint to extend session without re-login. This reduces UX friction while maintaining security.

Changes:
  1. Add /auth/refresh endpoint to issue new access tokens
  2. Update client SDK to auto-refresh expired tokens
  3. Add refresh token rotation for security
```

**Example 2: Bug Fix**
```
[fix] payment: fix duplicate callback causing inventory oversell

Summary:
  Add idempotency check before processing payment callback to prevent race condition when webhook is retried.

Changes:
  1. Add idempotency key validation in webhook handler
  2. Store processed callback IDs in Redis with TTL
  3. Return 200 immediately for duplicate callbacks
```

**Example 3: Performance**
```
[perf] database: optimize user list query to reduce N+1 problem

Summary:
  Use eager loading to fetch related roles in single query instead of N+1. This reduces query count from 101 to 2 for 100 users.

Changes:
  1. Add eager loading for roles relationship
  2. Remove N+1 queries in UserRepository
  3. Add index on user_roles table
```

**Example 4: Refactor**
```
[refactor] api-client: refactor HTTP client to support interceptors

Summary:
  Extract HTTP logic into separate interceptor chain. This enables easy retry logic and request logging without duplicating code.

Changes:
  1. Create Interceptor interface with onRequest/onResponse methods
  2. Implement RetryInterceptor with exponential backoff
  3. Move logging logic to LogInterceptor
```

## Common Types

| Type | Description |
|------|-------------|
| feat | New feature |
| fix | Bug fix |
| perf | Performance improvement |
| refactor | Refactoring (non-functional change) |
| test | Test related |
| docs | Documentation |
| chore | Build/tool/dependency update |
| style | Code formatting (no functional change) |
| ci | CI configuration |

## Notes

- Keep the first line within one line, no line breaks
- Use English for both description and Summary
- Be specific, avoid vague words ("some changes", "fix things")
