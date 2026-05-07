---
name: skill-manage
description: "Create new or fix/improve existing Claude Code skills. Use when: (1) User wants to create a new skill, (2) User asks 'how to make a skill' or 'create skill for X', (3) User says 'turn this into a skill', (4) User reports a skill isn't working or triggering incorrectly, (5) User says 'fix skill', 'update skill', 'tweak skill', 'skill not working', 'skill triggers too often'. Covers skill anatomy, creation workflow, frontmatter, bundled resources, trigger debugging."
---

# Skill Manage

Determine the mode from context:

- **Create**: user wants a new skill
- **Tweak**: user wants to fix or improve an existing skill

---

## Mode: Create

### Step 1: Understand with concrete examples

If the user says "turn this into a skill", extract tools used, step sequence, corrections, and input/output formats from conversation history before asking questions.

Otherwise, ask:

- What should the skill do?
- What would a user say to trigger it?
- What examples cover the main use cases?

### Step 2: Plan bundled resources

For each example, identify reusable assets:

- **Scripts** (`scripts/`) — code rewritten the same way each time
- **References** (`references/`) — schemas, API docs, domain knowledge
- **Assets** (`assets/`) — templates, images, boilerplate files

### Step 3: Initialize

```bash
python3 $HOME/.claude/skills/skill-manage/scripts/init_skill.py <skill-name> --path <parent-dir>
```

`--path` is the **parent** directory; the script appends `<skill-name>` automatically.

### Step 4: Edit the skill

**File structure:**

```
skill-name/
├── skill.md          ← required (SKILL.md also accepted)
├── scripts/
├── references/
└── assets/
```

**Frontmatter** — see [frontmatter.md](references/frontmatter.md) for all fields. Essentials:

- `name`: lowercase, hyphens
- `description`: what it does + when to use. Be slightly pushy — Claude undertriggers. Include near-miss triggers.

**Body:** instructions for another Claude instance. Include only non-obvious procedural knowledge.

**Path safety:** always use `$HOME/` not `~/` in file paths — `~` is not expanded outside interactive shells.

**Design patterns** — see [workflows.md](references/workflows.md) and [output-patterns.md](references/output-patterns.md).

**Keep SKILL.md under 500 lines.** Move detailed content to `references/`.

### Step 5: Test scripts

Run each script to verify it works. Fix bugs before finalizing.

### Step 6: Format

```bash
pnpm dlx @takazudo/mdx-formatter --write <path-to-skill.md>
```

---

## Mode: Tweak

### Step 1: Find and read the skill

Locations:

- `$HOME/.claude/skills/<name>/skill.md` (personal)
- `.claude/skills/<name>/skill.md` (project)

**macOS case-sensitivity:** check actual filename with `git ls-files --stage '<skill-dir>/'`. If both `SKILL.md` and `skill.md` exist in the index, remove the duplicate: `git rm --cached '<path>/SKILL.md'`.

### Step 2: Diagnose

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Skill not triggering | Poor `description` | Add clear keywords; make description pushy |
| Triggers too often | Description too broad | Narrow description; add `disable-model-invocation: true` |
| Skill not visible | Exceeds char budget | Run `/context`; shorten description or raise `SLASH_COMMAND_TOOL_CHAR_BUDGET` |
| Script fails | Bug or env issue | Read and run the script |

### Step 3: Apply changes

- **Adding a step:** renumber the entire sequence — no "Step 2.5" or "Step 0"
- **Rewriting description:** include what it does + trigger scenarios
- **Cutting:** remove instructions not pulling their weight; prefer explaining *why* over all-caps MUST/NEVER
- **Path safety:** replace any `~/` with `$HOME/`

Frontmatter reference — see [frontmatter.md](references/frontmatter.md).

### Step 4: Format

```bash
pnpm dlx @takazudo/mdx-formatter --write <path-to-skill.md>
```

### Step 5: Verify

1. YAML frontmatter parses without errors
2. Description matches intended trigger scenarios
3. Referenced files (scripts, references) exist
4. Body under 500 lines
5. No `~/` paths in file operations

---

## Skill Locations

| Location | Path | Scope |
| --- | --- | --- |
| Personal | `$HOME/.claude/skills/<name>/skill.md` | All projects |
| Project | `.claude/skills/<name>/skill.md` | This project only |
