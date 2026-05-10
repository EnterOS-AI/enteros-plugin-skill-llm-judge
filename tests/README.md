# Test Rationale — molecule-skill-llm-judge

## What this plugin does

`molecule-skill-llm-judge` is a prose-only skill: its logic lives entirely in
`skills/llm-judge/SKILL.md` (when to use, what inputs to send to the judge model,
scoring criteria). The adapter (`adapters/claude_code.py`) is a thin re-export of
`AgentskillsAdaptor` from `plugins_registry.builtins` — no business logic, no network
calls, no side effects.

## What is tested

- `plugin.yaml` is valid YAML with required fields (name, version, runtimes, skills)
- `skills/llm-judge/SKILL.md` has valid YAML frontmatter and a body with
  required sections (When to Use, Inputs, How to evaluate)
- `adapters/claude_code.py` exists and re-exports `AgentskillsAdaptor`
- `validate-plugin.py` (`.molecule-ci/scripts/`) exits zero

## What is NOT unit-tested (and why)

The LLM-as-judge evaluation logic requires calling an external model and comparing
output against criteria — testing it requires integration with a real workspace runtime.
Smoke tests cover the artifact structure; full evaluation requires integration tests.

## Running tests

```bash
python -m pytest tests/ -v
```
