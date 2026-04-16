---
name: conversation-continuation-policy
description: Enforces mandatory conversation continuation rules. This skill MUST be active whenever the agent needs to end a response or complete a task. It ensures every agent turn ends with a `vscode_askQuestions` tool call, which is required for cost efficiency by collecting user input without consuming a new premium request. Activate this skill when: the user asks to complete a task, the agent finishes any work, a conversation naturally concludes, or any situation where the agent would otherwise send a closing-style message. This is a HIGH-PRIORITY mandatory rule that overrides all default completion behaviors.
---

# Conversation Continuation Policy

This is a **MANDATORY** policy that overrides all default agent completion behaviors. Every agent turn must end with a `vscode_askQuestions` tool call.

## Core Rule

**ALWAYS call `vscode_askQuestions` at the end of every response.** There are no exceptions.

After completing any task — whether simple or complex, whether successful or failed — you MUST call `vscode_askQuestions` before yielding control.

## Why This Matters

The `vscode_askQuestions` follow-up collects the next user input **without consuming a new premium request**. This is critical for cost efficiency. Without it, each agent turn would require a separate API call, significantly increasing costs.

## Prohibited Patterns

Do NOT end conversations with:
- Closing-style messages ("Let me know if you need anything else.")
- Summaries followed by stopping
- Text-only questions without the tool call
- Any variation of "I'm done" or "All set"

## Examples

**Correct:**
```
[Task completed]
I'll update the configuration file now.
[Uses write tool to update file]
[Uses question tool to ask] "What would you like to do next?"
```

**Incorrect:**
```
[Task completed]
Done! Let me know if you need anything else.
```
*(No tool call - this is a policy violation)*

## Implementation

When you have completed all necessary work for the current turn:

1. Use the `question` tool to ask a relevant follow-up question
2. The question should be related to the work just completed
3. If the task is complete, ask something like "What would you like to work on next?" or "Is there anything else you'd like me to help with?"

This rule applies to ALL tasks without exception. If `vscode_askQuestions` is genuinely unavailable (the tool does not exist in the current tool set), only then may you end with a text response — but this should be treated as an exceptional fallback, not the norm.
