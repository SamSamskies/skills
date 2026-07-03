# AGENTS.md

Guidance for AI coding agents working in this repository.

## Repository Overview

This repo is a library of [Agent Skills](https://agentskills.io/) — packaged instructions that extend agent capabilities. Skills are discovered and installed via the [Skills CLI](https://skills.sh/).

## Creating a New Skill

### Directory Structure

```
skills/
  {skill-name}/           # kebab-case directory name
    SKILL.md              # Required: skill definition
    scripts/              # Optional: executable scripts
    references/           # Optional: supporting docs loaded on demand
```

### Naming Conventions

- **Skill directory**: `kebab-case` (e.g., `split-plan-into-slices`)
- **SKILL.md**: Always uppercase, always this exact filename
- **Scripts**: `kebab-case.sh` or `kebab-case.mjs`

### SKILL.md Format

Every skill requires YAML frontmatter and a markdown body:

```markdown
---
name: {skill-name}
description: {One sentence: what it does and when to use it. Include trigger phrases.}
---

# {Skill Title}

{Instructions for the agent.}
```

The `name` field must match the directory name. The `description` is critical — agents use it to decide when to activate the skill.

### Best Practices

- **Keep SKILL.md under 500 lines** — put detailed reference material in `references/`
- **Write specific descriptions** — include both WHAT the skill does and WHEN to use it
- **Use progressive disclosure** — link supporting files from SKILL.md; agents read them only when needed
- **Prefer scripts over inline code** — script execution does not consume context (only output does)
- **File references one level deep** — link directly from SKILL.md to supporting files

### After Adding a Skill

1. Add an entry to `README.md` under **Available Skills**
2. Verify install works: `npx skills add . --skill {skill-name}`

## End-User Installation

```bash
npx skills add <owner>/skills --skill {skill-name}
```

For manual installs:

**Cursor:**
```bash
cp -r skills/{skill-name} ~/.cursor/skills/
```

**Claude Code:**
```bash
cp -r skills/{skill-name} ~/.claude/skills/
```
