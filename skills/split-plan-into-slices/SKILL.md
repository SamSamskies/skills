---
name: split-plan-into-slices
description: Splits a Cursor Plan Mode markdown plan into multiple independent vertical-slice plan files, each buildable and testable on its own. Use when a generated plan has TODOs spanning multiple layers or features that should be built, reviewed, and committed independently rather than all at once.
---

# Split Plan Into Vertical Slices

## When to Use

- The user has an existing Plan Mode output (a markdown plan with a TODO
  checklist) and wants to build it incrementally instead of clicking Build once
  for the whole thing.
- The plan covers more than one end-to-end capability (e.g. "auth" and
  "billing" and "notifications" all in one plan).
- The user says things like "split this plan into slices," "break this into
  separate plans," or "I want to build this one vertical slice at a time."

## Instructions

1. **Read the source plan file.** Identify every TODO item and the files/code
   references it touches.

2. **Group TODOs by vertical slice, not by layer.** A slice is an
   end-to-end, user-facing (or system-facing) capability — not "all backend
   work" followed by "all frontend work." Each slice should independently
   touch whatever layers (data, API, UI) it needs to be a complete, testable
   unit on its own.

3. **Respect dependency order.** If slice B needs a shared type, schema, or
   utility introduced in slice A, put A first and note the dependency at the
   top of B's plan file. Prefer minimizing cross-slice dependencies where
   reasonably possible — if a "shared foundation" chunk is unavoidable, call
   it out explicitly as its own slice built first (e.g.
   `plan-00-shared-foundation.md`).

4. **Every slice must leave the app in a working, buildable state.** No slice
   should be merged/built if it leaves the project in a broken or
   non-compiling state, even before later slices land.

5. **Write one file per slice** to `.cursor/plans/`, named
   `plan-<slice-name>.md` (or `plan-00-<name>.md`, `plan-01-<name>.md`, etc.
   if strict build order matters). Each file should contain:
   - A one-line description of the capability this slice delivers
   - Any dependency note ("Requires plan-00-shared-foundation to be built
     first")
   - Only the TODOs, file paths, and code references relevant to this slice
   - Nothing about slices other than the ones it explicitly depends on

6. **Do not build or edit any code yet.** This skill only produces the split
   plan files. Building happens in a separate agent session per slice, kept
   isolated so context stays focused per slice.

7. **Summarize the split for the user** after writing the files: list each
   slice file name, one-line description, and build order. Let the user
   confirm before they start building the first slice.

## Notes

- These plan files are scratch artifacts, not permanent documentation.
  Default to treating them as disposable — delete or fold into commit
  messages/PR descriptions once a slice ships, rather than committing them
  long-term.
- If the original plan doesn't cleanly decompose (e.g. it's already a single
  small feature), say so instead of forcing an artificial split.
