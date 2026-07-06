---
name: draft-pr
description: Drafts a gh pr create command for user review before opening a pull request. Use when the user asks to draft a PR, prepare a PR command, review a PR before creating it, or attaches @Branch (Diff with Main). Does not create the PR until the user explicitly approves.
---

# Draft PR

Prepare a pull request command for review. **Do not run `gh pr create` until the user approves.**

## When to Use

- Branch is pushed and the user wants a PR drafted, not created yet
- The user attaches `@Branch (Diff with Main)` for PR prep
- The user says "draft a PR", "prepare the PR command", or "review before creating the PR"

## Instructions

1. **Confirm the branch is pushed.** Run in parallel:
   - `git status`
   - `git branch -vv` (check upstream tracking)
   - `git rev-parse --abbrev-ref HEAD`

   If the branch has no upstream or is ahead of remote, push first (`git push -u origin HEAD`) and tell the user. Do not draft until the branch is on the remote.

2. **Read the diff against main.** Prefer the attached `@Branch (Diff with Main)` when provided. Otherwise run in parallel:
   - `git log main...HEAD --oneline` (or `master` if that is the default branch)
   - `git diff main...HEAD`

   Use the repo's default branch (`main` or `master`) as the base.

3. **Draft the PR content** from the full branch diff and commit history — not just the latest commit:
   - **Title**: conventional commit style — `type(scope): short imperative description` (e.g. `feat(auth): add JWT login endpoint`, `fix(reports): correct timezone formatting`). Pick the type and scope that best fit the overall change.
   - **Body**: use exactly these sections:

     ```markdown
     ## Summary
     <1-3 bullet points covering the full branch change>

     ## Test plan
     <Checklist of concrete verification steps>
     ```

4. **Present for review** in this order:

   **Preview** — show the title and body rendered so the user can read them easily.

   **Command** — one copy-pastable `gh pr create` command using a HEREDOC for the body:

   ```bash
   gh pr create --title "type(scope): description" --body "$(cat <<'EOF'
   ## Summary
   - ...

   ## Test plan
   - [ ] ...

   EOF
   )"
   ```

5. **Stop and wait.** Ask the user to review the title, body, and command. Only run `gh pr create` when they explicitly approve (e.g. "looks good", "create it", "run it"). If they request edits, revise and re-present.

## Rules

- Draft only by default — never create the PR without explicit approval
- Analyze all commits on the branch, not just the most recent one
- Do not push to remote unless needed to get the branch uploaded first
- Do not amend, rebase, or change git history as part of this skill
- Keep the summary focused on *why*; keep the test plan actionable

## Example

User: `Branch is pushed. Draft one-line gh pr create command: --title conventional commit style --body with ## Summary and ## Test plan @Branch (Diff with Main)`

Agent reads the diff, then replies with preview + HEREDOC command, and ends with something like: "Review the title and body above. Say when to run the command or what to change."
