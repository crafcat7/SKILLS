---
name: conversation-continuation-policy
description: Enforces the follow-up-question turn-closure policy. This skill must be active whenever the agent is about to end a response — whether the task succeeded, failed, or naturally concluded. Use this skill when the user asks to complete a task, the agent finishes any work, a conversation reaches a lull, or any situation where the agent would otherwise send a plain closing message. It requires every agent turn in an interactive editor session to end with a follow-up-question tool call (the `question` tool in OpenCode, or `vscode_askQuestions` in VSCode/Copilot integrations) so the next user input is collected inside the current premium request window rather than triggering a new billed request. Make sure to use this skill at the end of every interactive agent turn, even when the agent believes the conversation is "done".
---

# Conversation Continuation Policy

Agent turns that end with plain closing text force the platform to open a new premium request for the next user message. Ending with a follow-up-question tool call instead keeps the exchange inside the current request window, which substantially reduces cost in interactive editor sessions. This policy makes that behavior the default.

## Core rule

End every interactive agent turn with a call to the follow-up-question tool.

- In OpenCode, that tool is named **`question`**.
- In VSCode / Copilot integrations, the equivalent is `vscode_askQuestions`.

Pick whichever exists in the current tool list and use it consistently within a session. The two are interchangeable for the purpose of this policy.

## When to apply

Apply this policy whenever:

- The agent has finished the work requested in the current turn
- A task succeeded, failed, or was blocked
- The conversation has naturally reached a pause

## When *not* to apply

Do not force a question-tool call if any of the following holds:

- No question-style tool is present in the current tool list
- The session is clearly non-interactive (batch run, automated pipeline, CI)
- The user has explicitly asked the agent to stop producing follow-up questions

In those cases ending with a plain text response is acceptable. These are exceptions, not the default.

## What the follow-up should ask

The question should be *useful* — a genuine choice that helps the user steer the next turn. Bad defaults to avoid:

- "Is there anything else I can help with?" — empty filler
- Yes/no confirmations of something already done
- Questions that duplicate information already in your response

Good follow-ups:

- Offer 2-4 concrete next actions relevant to the work just finished
- Confirm an ambiguous choice that was deferred
- Invite the user to inspect or test the result

## Examples

**Correct — task finished, offering concrete next steps**

> I've updated `config.yaml` and verified the new value is picked up by the build.
> *(calls the question tool with options: "Run the test suite", "Commit the change", "Review other config values", "Done for now")*

**Incorrect — plain closing text, policy violation**

> Done! Let me know if you need anything else.
> *(no tool call — the next user message will open a new billed request)*

**Correct — fallback when no question tool exists**

> (In an environment where `question` / `vscode_askQuestions` is not available)
> I've updated the file. Let me know what you'd like to do next.

## Implementation notes

1. After completing any substantive work, draft 2-4 plausible next actions as options for the follow-up question.
2. The question text should reference the work just done so the options feel grounded, not generic.
3. If you find yourself repeatedly asking the same filler question across turns, revisit whether the task is actually finished or whether you should instead surface a concrete decision to the user.
