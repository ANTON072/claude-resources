---
name: subagent-manage
description: "Create new or fix/improve existing Claude Code custom agents (subagents). Use when: (1) User wants to create a new custom agent, (2) User says 'create agent', 'new agent', 'make subagent', (3) User wants a specialized agent for delegation, (4) User reports an agent isn't working well, (5) User says 'fix agent', 'update agent', 'tweak agent', 'agent not working'. Covers agent file format, YAML frontmatter, tool restrictions, model selection, trigger debugging."
---

# Subagent Manage

Determine the mode from context:

- **Create**: user wants a new agent
- **Tweak**: user wants to fix or improve an existing agent

---

## Mode: Create

### Step 1: Understand the agent's purpose

Ask if not clear from context:

- What tasks should this agent handle?
- Personal (`$HOME/.claude/agents/`) or project-scoped (`.claude/agents/`)?
- Read-only or does it need write access?

### Step 2: Choose settings

**Model:**

- `opus` — complex reasoning, code review, architecture
- `sonnet` — general development, balanced
- `haiku` — fast simple tasks, formatting, lookups

**Tool restrictions:**

- Read-only: `tools: Read, Grep, Glob`
- No web: `disallowedTools: WebFetch, WebSearch`
- Full access: omit `tools` (inherits all)

**Key constraints:**

- Subagents CANNOT spawn other subagents
- Keep body focused — it becomes the agent's system prompt
- Use `$HOME/` not `~/` in all file paths

### Step 3: Create the agent file

```markdown
---
name: agent-name
description: One sentence describing when Claude should delegate to this agent
model: sonnet
tools: Read, Grep, Glob, Bash
---

You are a specialized [role]. [Core instruction in 1-2 sentences.]

## Responsibilities

[What this agent does — keep concise]

## Workflow

[Step-by-step procedure if applicable]
```

### Step 4: Format

```bash
pnpm dlx @takazudo/mdx-formatter --write <path-to-agent-file.md>
```

### Step 5: Verify

1. File at the correct location
2. YAML frontmatter parses correctly
3. Description clearly indicates when to use the agent

---

## Mode: Tweak

### Step 1: Find and read the agent

Locations:

- `$HOME/.claude/agents/*.md` (personal)
- `.claude/agents/*.md` (project)

### Step 2: Diagnose

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Agent not being used | Poor `description` | Rewrite with clear trigger keywords |
| Agent too slow | Wrong model | Switch to `sonnet` or `haiku` |
| Agent can't edit files | `tools` missing Write/Edit | Add needed tools |
| Agent doing too much | No tool restrictions | Add `tools:` or `disallowedTools:` |
| Agent forgets context | No persistent memory | Add `memory: user` or `memory: project` |
| Agent runs too long | No turn limit | Add `maxTurns: N` |

### Step 3: Apply changes

Edit the agent's Markdown file. `description` is the most impactful field.

**Frontmatter fields:**

| Field | Description |
| --- | --- |
| `name` | Identifier (lowercase, hyphens) |
| `description` | When to use (primary delegation trigger) |
| `model` | `opus`, `sonnet`, `haiku`, `inherit` |
| `tools` | Tool allowlist |
| `disallowedTools` | Tool denylist |
| `permissionMode` | `default`, `acceptEdits`, `delegate`, `dontAsk`, `bypassPermissions`, `plan` |
| `maxTurns` | Max agentic turns |
| `skills` | Preloaded skills |
| `memory` | `user`, `project`, `local` |

### Step 4: Format

```bash
pnpm dlx @takazudo/mdx-formatter --write <path-to-agent-file.md>
```

### Step 5: Verify

1. YAML frontmatter has no syntax errors
2. Tool restrictions match intended behavior
3. Description contains keywords users would naturally say

---

## Tips

- Prefer `disallowedTools` over `tools` if only blocking a few tools
- Subagents cannot nest — don't add `Task` to agents that will be spawned as subagents
- `description` is the primary trigger mechanism — most impactful field to improve
