---
name: error-log-analyzer
description: |
  Analyzes error logs and fixes issues in code projects. Use this skill when the user provides an error log (compilation errors, runtime exceptions, test failures, build failures) and wants help diagnosing and fixing the problem. The skill works with any programming language or framework. It performs root cause analysis, proposes fixes for user approval, then implements them and generates a detailed repair report. Always use this skill when users mention error logs, stack traces, build failures, compilation errors, or want help debugging issues in their codebase.
---

# Error Log Analyzer

Analyze error logs, diagnose issues, propose fixes, and generate repair reports.

## Workflow

### 1. Gather Information

Collect from the user:
- **Project directory**: The root path of the codebase
- **Error log**: The error output (paste directly or file path)

If the log is in a file, read it immediately. If pasted in chat, work with it directly.

### 2. Analyze the Error Log

Parse the log to extract:
- **Error type**: Compilation, runtime, test failure, build error, etc.
- **Error messages**: The actual error text
- **File locations**: Paths and line numbers mentioned in the stack trace
- **Error sequence**: Order of errors (some may be cascading from a root cause)

Identify the **root cause** — often the first error or the one that triggers cascading failures. Explain your analysis to the user before proposing fixes.

### 3. Inspect the Codebase

For each error location:
1. Read the relevant files (use line ranges from the error)
2. Understand the context — imports, surrounding code, dependencies
3. Check for related issues in nearby code

Use `grep` and `glob` to find:
- Missing imports or files
- Incorrect function signatures
- Type mismatches
- Configuration issues

### 4. Propose Fixes

For each issue found, explain:
- **What's wrong**: Clear description of the problem
- **Why it's wrong**: Root cause analysis
- **How to fix**: Specific changes needed
- **Risk level**: Low (cosmetic), Medium (logic change), High (structural change)

Present fixes in a clear list and **wait for user confirmation** before making changes.

Format your proposal like this:

```
## Proposed Fixes

### Fix 1: [Brief description]
- **File**: path/to/file.js:42
- **Problem**: [What's wrong]
- **Solution**: [What to change]
- **Risk**: Low/Medium/High

### Fix 2: [Brief description]
...

Do you want me to proceed with these fixes?
```

### 5. Apply Fixes (After User Approval)

Once the user confirms:
1. Apply each fix using `edit` tool (prefer over `write` when possible)
2. Keep changes minimal — fix only what's needed
3. Preserve existing code style and patterns
4. Run verification if possible (build, test, lint)

### 6. Generate Repair Report

Create a `repair-report.md` in the project directory with this structure:

```markdown
# Repair Report

**Date**: [ISO timestamp]
**Project**: [directory path]
**Error Type**: [Compilation/Runtime/Test/Build]

## Summary

[Brief overview of what was found and fixed]

## Errors Analyzed

### Error 1: [Error type]
- **Location**: [file:line]
- **Message**: [error message]
- **Root Cause**: [why this happened]

### Error 2: [Error type]
...

## Fixes Applied

### Fix 1: [Description]
- **File**: [path:line]
- **Before**: [code snippet or description]
- **After**: [code snippet or description]
- **Reasoning**: [why this fix works]

### Fix 2: [Description]
...

## Verification

[Results of any verification performed - build output, test results, etc.]

## Prevention Recommendations

1. [Suggestion to prevent similar issues]
2. [Suggestion]
...

## Files Modified

- [file path 1]
- [file path 2]
...
```

## Guidelines

- **Be thorough**: Read enough context to understand the issue fully
- **Be conservative**: Don't make unnecessary changes; fix the reported issue
- **Explain clearly**: Use simple language, especially for non-obvious fixes
- **Check dependencies**: Verify that fixes don't break other parts of the code
- **Preserve style**: Match the existing code's formatting and conventions
- **Multiple languages**: Adapt your approach to the project's language/framework

## Error Log Formats

The skill handles various log formats:

**Compilation errors** (TypeScript, C++, Rust, etc.):
```
src/utils.ts:42:5 - error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

**Runtime exceptions** (Python, Java, JavaScript, etc.):
```
Traceback (most recent call last):
  File "app.py", line 15, in main
    result = process_data(data)
TypeError: 'NoneType' object is not iterable
```

**Test failures** (Jest, pytest, JUnit, etc.):
```
FAIL src/components/Button.test.tsx
  ● Button › renders correctly

    Expected: "Click me"
    Received: "Submit"
```

**Build errors** (Webpack, Gradle, Cargo, etc.):
```
ERROR in ./src/index.ts
Module not found: Error: Can't resolve './missing-module'
```

Parse these patterns to extract file paths, line numbers, and error messages automatically.
