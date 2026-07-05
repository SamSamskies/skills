# Agent Skills

A library of agent skill workflows for AI coding agents. Each skill is a directory with a `SKILL.md` file containing instructions the agent loads on demand.

Skills follow the [Agent Skills](https://agentskills.io/) format and install with the [Skills CLI](https://skills.sh/).

## Available Skills

### review-scope

Read-only scope check on uncommitted changes against the current task. Fast pre-commit gate — bullets for out-of-scope edits, or `clean.`

**Use when:**

- You want to know if the diff wandered beyond the task
- You attach `@Commit (Diff of Working State)` for a scope check
- You say things like "review scope only" or "anything beyond the task?"

### split-plan-into-slices

Splits a Cursor Plan Mode markdown plan into multiple independent vertical-slice plan files, each buildable and testable on its own.

**Use when:**

- A Plan Mode output has TODOs spanning multiple layers or features
- You want to build, review, and commit one vertical slice at a time
- You say things like "split this plan into slices" or "break this into separate plans"

## Installation

Install all skills from this repo:

```bash
npx skills add SamSamskies/skills
```

Install a single skill:

```bash
npx skills add SamSamskies/skills --skill split-plan-into-slices
```

Install globally (available in every project):

```bash
npx skills add SamSamskies/skills -g
```

## Usage

Once installed, skills are available to your agent automatically. The agent reads the skill name and description at startup and loads full instructions when a task matches.

**Examples:**

```
Split this plan into vertical slices
```

```
Break this Plan Mode output into separate buildable plans
```

## Adding a Skill

1. Create a directory under `skills/` using kebab-case:

   ```bash
   mkdir -p skills/my-skill
   ```

2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`) and markdown instructions. Or scaffold a template:

   ```bash
   npx skills init my-skill --path skills/my-skill
   ```

3. Update this README with a short entry for the new skill.

See [AGENTS.md](AGENTS.md) for authoring conventions used in this repo.

## Skill Structure

```
skills/
  {skill-name}/
    SKILL.md              # Required: skill definition
    scripts/              # Optional: executable helpers
    references/           # Optional: docs loaded on demand
```

## License

MIT
