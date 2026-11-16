# pstack in jcode

Upstream: https://github.com/backnotprop/pstack (mirror of cursor/plugins/pstack). Fork: xenking/pstack, branch `jcode`, checkout `~/.jcode/pstack`.
Each installed skill dir has `.pstack-upstream` (commit + original name). 55 skills.

## Adaptations
- `docs/harness/jcode.md`: maps Cursor Task/AskQuestion/models/paths to jcode `swarm`/`ask`/`todo`/allowlist. Every SKILL.md gets a pointer line to it.
- Renamed on collision: pstack `tdd` -> `/tdd-bugfix`, `teach` -> `/teach-work` (cross-links rewritten).
- Model roles: `~/.agents/pstack-models.md` (swarm pool only: gpt-6.1-sol/terra, gpt-6-luna; judgment = coordinator).

## Start
- `/poteto-mode <task>` for rigorous work. `/poteto-help` to pick a skill.
- Verification: `/verify-jcode`, `/verify-jcode-desktop` (project skills in each repo).

## Re-sync
`~/.jcode/bin/sync-pstack-skills --pull` (merges upstream main into `jcode`, recopies; idempotent). Then `skill_manage reload_all`.

## Enforcement hooks (pre_tool, ~/.jcode/config.toml)
- `~/.jcode/hooks/git-guardrails`: push only to github.com/xenking/*, no force. Freeze all pushes: `touch ~/.jcode/push-freeze` (remove to unfreeze). Tests: `bash ~/.jcode/hooks/git-guardrails.test`.
- `~/.jcode/hooks/verify-gate`: a code push to xenking/jcode or xenking/jcode-desktop needs a passing, cleaned `/verify-jcode` or `/verify-jcode-desktop` receipt whose commit already contains every changed file. Docs-only (*.md, .jcode/skills) exempt. Override: `touch ~/.jcode/push-verify-skip`. Tests: `bash ~/.jcode/hooks/verify-gate.test`.
