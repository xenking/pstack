# pstack in jcode

pstack skills use Cursor names. In jcode, read this file once per session, then map each Cursor name with the tables below. `<pstack>` is `~/.jcode/pstack`. Skills are symlinked into `~/.jcode/skills/<name>`, so `/name` and the `skill` tool both resolve them. If a Cursor field has no row, drop it and say so.

## Tools

| Cursor | jcode |
| --- | --- |
| `Task` | `swarm` with `action: "spawn"`. The `label` names the worker's todo lane, `prompt` carries the brief, `model` picks the model. Drain with `swarm` `action: "await_members"`. |
| `AskQuestion` | `ask`. Label the recommended option `(Recommended)`. |
| Open a todolist | `todo`. Playbook steps go in verbatim as items. |
| Run another pstack skill | `/name`, or `read` `<pstack>/skills/<name>/SKILL.md` and follow it. |
| `Grep`, `Glob` | `agentgrep` (`grep`, `find`, `outline`). |
| Many dependent tool calls | `codemode`. Only its return value reaches context. |
| `/loop` | `bash` with `run_in_background` plus `bg` `action: "wait"`, or the `schedule` tool for timed wake-ups. |

## Subagent fields

| Cursor | jcode |
| --- | --- |
| `subagent_type: generalPurpose` | Plain `swarm` spawn. |
| `poteto-agent` | Plain spawn whose `prompt` starts with "Read `<pstack>/skills/poteto-mode/SKILL.md` in full, including its Principles section, before any work." |
| `comment-sicko` | Plain spawn whose `prompt` is `<pstack>/skills/no-comments/references/comment-sicko.md` verbatim, then the scope. |
| `readonly: true` | Say "do not edit files" in the brief, then check `git status` afterward. |
| `environment: "cloud"` | Local worker in its own worktree, `~/.jcode/wt/<name>-<repo>`, off `origin/master`. Pass it as `working_dir`. Never two workers in one checkout. |
| `run_in_background: true` | Omit. Swarm workers already run in the background and report back. |
| `resume` | Fresh spawn with the earlier report pasted into the brief. `swarm` `action: "message"` only when the worker holds live state. |
| `file_attachments` | List the paths in the brief. |

## Models

Only the models in `~/.jcode/config.toml` `[agents].swarm_model_allowlist` can be spawned. A spawn with any other model fails, so never pass a Claude or Gemini slug. The role lines live in `~/.agents/pstack-models.md`. Read it at task start. Each value is `<jcode model> <effort>`, such as `openai:gpt-6.1-sol xhigh`. Pass the model as `model`. jcode has no per-spawn effort field. The worker runs at `[agents].swarm_effort`, capped by `swarm_effort_caps`. State the wanted effort in the brief and pick sol for the hardest roles. Drop `-fast` everywhere.

Panel diversity comes from different models in the pool. Where a panel asks for a non-Claude family, use another pool model. The coordinator chat itself is a different family (Claude), so the coordinator's own pass counts as one more panel voice when a skill asks for cross-vendor judgment.

## Paths

| Cursor | jcode |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.jcode/skills/` (project), `~/.jcode/skills/` (user). jcode also reads `.claude/skills/` and `.agents/skills/`. |
| `.cursor/rules/*.mdc` | `AGENTS.md` in the repo, `~/AGENTS.md` globally. |
| `~/.cursor/rules/pstack-models.mdc` | `~/.agents/pstack-models.md`. |
| Transcripts | `~/.jcode/sessions/session_*.json` (keys `working_dir`, `messages`) and live `*.journal.jsonl`. Keep only files whose `working_dir` equals the workspace. `session_search` and `conversation_search` query the same store. |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | `<pstack>/<path>`. |

## Tools pstack doesn't ship

| Cursor | jcode |
| --- | --- |
| `create-skill` | `/writing-for-agents`. |
| `deslop` | `/no-ai-slop`, then reread the diff. |
| `control-cli` | The repo's `verify-*` skill. For jcode itself, `/verify-jcode`, plus `debug_socket` tester instances and frames. |
| `control-ui` | `/verify-jcode-desktop` for Jcode Desktop. Web UIs use agent Chrome CDP on port 9333 (`agent-chrome`), never 9222. |
| `tdd` (pstack) | Installed as `/tdd-bugfix`. `/tdd` stays the existing red-green skill. |
| `teach` (pstack) | Installed as `/teach-work`. `/teach` stays the existing skill. Where a pstack skill says **teach**, use `/teach-work`. |

## Cursor-only features

When a step needs Cursor Automations, Bugbot setup, or a Cursor cloud agent dashboard, write one line, `Skipped in jcode: <step>. <reason>.`, and continue. Bugbot-style review on GitHub maps to `/codex-pr-review-gate`. A built-in PR tool maps to `gh pr create`.

## Repo rules that win over pstack

- Work on `master`. Commit and push as you go. Never force-push.
- Rust builds go through `justrust` (`check`, `clippy`, `test`). jcode self-dev builds go through `selfdev`.
- Replies to the user are in Russian and terse. The reply rules of poteto-mode still apply to structure.
