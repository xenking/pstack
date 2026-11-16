# pstack model configuration for jcode. One line per role. Delete a line to fall back to the skill default.
# Values are `<jcode model> <effort>`. Only models in [agents].swarm_model_allowlist can spawn.
# `inherit-parent` runs the role in the coordinator chat (Claude) instead of a worker.
# defaults: 2
# budget: large (xhigh)
feature, refactoring: openai:gpt-6.1-sol xhigh
bug-fix: openai:gpt-6.1-sol xhigh
perf-issue: openai:gpt-6.1-sol xhigh
hillclimb: openai:gpt-6.1-sol xhigh
judgment and prose: inherit-parent
hardest tasks: openai:gpt-6.1-sol xhigh
reflect tooling: openai:gpt-6.1-terra high
reflect judgment, divergent, synthesizer: openai:gpt-6.1-sol xhigh
arena runners: openai:gpt-6.1-sol xhigh, openai:gpt-6.1-terra high, inherit-parent
arena cross-judge pool: openai:gpt-6.1-sol xhigh, openai:gpt-6.1-terra high
swarm workers: openai:gpt-6.1-terra high
architect runners: openai:gpt-6.1-sol xhigh, openai:gpt-6.1-terra high, inherit-parent
interrogate reviewers: openai:gpt-6.1-sol xhigh, openai:gpt-6.1-terra high, openai:gpt-6-luna high
