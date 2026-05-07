# Global Instructions

## Tools & Runtime

- Check the project's package manager (pnpm, npm, etc.) before running install or script commands
- `gh` CLI for GitHub operations (PRs, issues, API)

## Code Style

- Prefer kebab-case for file names to avoid case-sensitivity issues, unless the project uses a different convention

## Code Comments — Capture Hidden Spec/Context

- Default is **no comments** — well-named identifiers should explain what the code does.
- Exception: when code embeds knowledge that lives **outside the codebase** — a product spec, an external API contract, a magic value imposed by a partner service, an undocumented platform quirk — leave a short comment. A future reader cannot recover this context by reading more code, and the source-of-truth document may be scattered, behind a login, or nowhere written down.
- Concrete case: a frontend payload field like `bdidso: 10` is opaque on its own. Even if a spec exists somewhere, the next reader has no idea where to look. A one-liner — `// product-specific param required by API X; value is fixed by spec` — saves them from a fruitless search.
- Test before adding: *"If I delete this comment, can a reasonable reader recover the meaning from code, identifiers, or obvious docs?"* If no, the comment earns its place.
- Keep it to a single line of **why** / **where it comes from** — not a tutorial, not a re-statement of what the code already says.

## Git Safety

- No force push, no `--amend` unless explicitly permitted
- No branch name reuse. Regular merge by default (not squash)

## Testing & Verification

- Unit tests alone cannot prove visual correctness. If the change is UI/CSS/layout, verify with computed styles or screenshots.
- When user says "it's still broken" after you tested, escalate to a deeper testing level -- do not re-run the same test
- **NEVER suggest "clear browser cache" or "hard refresh" as a solution.** If the user says it's still broken, the code is still broken. Investigate the actual cause instead of blaming cache.

## GitHub Issues

- When creating a GitHub issue that needs images (screenshots, diagrams, etc.), use `/gh-issue-with-imgs` to upload images as release assets and embed them in the issue body. `gh issue create` cannot attach images natively.

## Safety

- `rm -rf`: relative paths only (`./path`, never `/absolute/path`)
- Worktree prompt files and truly ephemeral temp files stay in `__inbox/` (gitignored)
