---
name: review-scope
description: Read-only scope check on uncommitted changes against the current task. Use when the user asks "anything beyond the task?", "review scope only", "scope review", "did we stay in scope?", or attaches @Commit (Diff of Working State). Reply with bullets or "clean." Never edit files.
---

# Review Scope

Lightweight pre-commit gate: did the working-tree diff stay within the current task?

This is **not** a bug review, code quality review, or spec validation. Do not launch subagents. Do not edit files.

## When to Use

- After implementing a task, before committing
- When the user attaches `@Commit (Diff of Working State)` for a scope check
- When the user says "anything beyond the task?", "review scope only", or "did we wander?"

## Instructions

1. **Infer the task** from the conversation — the user's original request, plan slice, or issue being worked on. If unclear, state what you assumed.

2. **Read the diff** — prefer the attached working-state diff when provided. Otherwise use staged + unstaged changes:
   - `git diff` (unstaged)
   - `git diff --cached` (staged)
   - Include both when mixed.

3. **Check scope only.** For each changed file or hunk, ask: is this required by the task, or drive-by/unrelated work?

   Flag as out of scope:
   - Unrelated file edits (formatting, refactors, docs) not needed for the task
   - Extra features or behavior beyond what was asked
   - Unrequested dependency or config changes

   Do **not** flag:
   - Changes that directly implement the task, even if imperfect
   - Necessary supporting changes (imports, types, tests for the task)
   - Bug fixes in code you had to touch to complete the task

4. **Reply in one of two formats only:**
   - If everything is in scope: `clean.`
   - If anything is out of scope: a bullet list. One bullet per out-of-scope item — file path and brief reason.

## Output Rules

- Read-only — never edit, revert, or fix files
- No severity tables, no subagents, no standards pass
- No commentary beyond the bullets or `clean.`
- Do not review correctness — that is for bug/security review skills

## Example

Task: "Add retry to the payment API call."

```
• README.md — doc rewrite unrelated to payment retry
• package.json — bumped lodash; not required for retry logic
```

If only `payment.ts` changed with retry logic: `clean.`
