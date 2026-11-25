# pstack model configuration for jcode. One line per role. Delete a line to fall back to the skill default.
# Values are `<jcode model> <effort>`. Only models in the swarm tool's `model` enum can spawn.
# `inherit-parent` runs the role in the coordinator chat instead of a worker.
# gpt-6.2-sol is capped at high by [agents].swarm_effort_caps.
# Routing from the 2026-11-25 session audit: sol had 7/10 runtime-verified workers and no scope drift.
# defaults: 3
# budget: large (xhigh)
feature, refactoring: openai:gpt-6.2-sol high
bug-fix: openai:gpt-6.2-sol high
perf-issue: openai:gpt-6.2-sol high
hillclimb: openai:gpt-6.2-sol high
judgment and prose: inherit-parent
hardest tasks: claude:claude-opus-5-5 xhigh
reflect tooling: openai:gpt-6.1-terra high
reflect judgment, divergent, synthesizer: openai:gpt-6.2-sol high
arena runners: claude:claude-opus-5-5 high, openai:gpt-6.2-sol high, opencode-go:deepseek-v4-pro high
arena cross-judge pool: claude:claude-opus-5-5 high, openai:gpt-6.2-sol high
swarm workers: openai:gpt-6.1-terra high
architect runners: claude:claude-opus-5-5 high, openai:gpt-6.2-sol high, opencode-go:deepseek-v4-pro high
interrogate reviewers: claude:claude-opus-5-5 high, openai:gpt-6.2-sol high, opencode-go:deepseek-v4-pro high
