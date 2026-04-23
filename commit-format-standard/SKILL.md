---
name: commit-format-standard
description: Enforces a standardized Git commit message format. Use this skill whenever the user is about to commit code — including phrases like "commit this", "commit these files", "write the commit message", "generate a commit", "git commit", "make a commit", "help me commit", or any scenario involving version-control commit operations. This skill ensures every commit contains a clear type/scope/description first line plus a Summary and Changes list, so teammates and automation tooling (changelog generators, semantic-version inference, code review bots) can parse commit history reliably. Make sure to use this skill whenever the user is about to commit code, even if they do not explicitly ask for a specific format.
---

# Commit Format Standard

Every commit message must follow the structure below. The reason for this structure is that teammates and automation tooling (changelog generators, semantic-version inference, code review bots, release automation) need a predictable shape to parse. A consistent format also makes it much faster for a human reviewer to scan history and understand intent.

## Language

Write the entire commit message in **English**, including the first line, Summary, and Changes items. Even if the user is conversing in another language, translate their change description into clear, natural English technical prose — do not transliterate.

## Format template

```
[<type>] <scope>: <description>

Summary:
  <detailed explanation, including purpose, solution, and impact>

Changes:
  1. <change point 1>
  2. <change point 2>
  3. <change point 3>
```

## Format specification

### First line: `[<type>] <scope>: <description>`

| Component | Rule |
|-----------|------|
| `type` | Lowercase: feat, fix, refactor, test, docs, chore, perf, etc. |
| `scope` | Module or component name (optional) |
| `description` | Verb first, brief change description, within 50 characters |

### Summary

- Indent with 2 spaces
- Explain the *what*, *why*, and *impact*
- Typically 1-3 sentences

### Changes

- Use a numbered list (1. 2. 3.)
- Each item is a specific, independent change
- One sentence per item
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
  Use eager loading to fetch related roles in a single query instead of N+1. This reduces query count from 101 to 2 for 100 users.

Changes:
  1. Add eager loading for roles relationship
  2. Remove N+1 queries in UserRepository
  3. Add index on user_roles table
```

**Example 4: Refactor**
```
[refactor] api-client: refactor HTTP client to support interceptors

Summary:
  Extract HTTP logic into a separate interceptor chain. This enables retry logic and request logging without duplicating code.

Changes:
  1. Create Interceptor interface with onRequest/onResponse methods
  2. Implement RetryInterceptor with exponential backoff
  3. Move logging logic into LogInterceptor
```

## Common types

| Type | Description |
|------|-------------|
| feat | New feature (high frequency) |
| fix | Bug fix (high frequency) |
| refactor | Refactoring — no behavior change (high frequency) |
| perf | Performance improvement |
| test | Test related |
| docs | Documentation |
| chore | Build / tooling / dependency update |
| style | Code formatting only (no functional change) |
| ci | CI configuration |

When in doubt between two types, prefer the one that better describes the *user-visible* effect (`feat` over `refactor` if behavior changes, `fix` over `refactor` if a bug is resolved).

## Notes

- The first line must be a single line with no line breaks
- Keep the entire message in English (see "Language" above)
- Be specific — avoid vague wording such as "some changes" or "fix things"
- If a commit genuinely spans multiple unrelated concerns (e.g. `feat` + `fix` for different modules), prefer splitting into separate commits rather than mixing types in one message
