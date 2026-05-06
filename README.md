# molecule-skill-llm-judge — LLM-as-Judge Gate

Plugin for Claude Code. Scores whether an agent's deliverable (a PR, a delegation
result, a generated config) actually addresses the original request — the failure mode
unit tests miss.

## The problem it solves

Unit tests verify the code *ran*. They don't verify it did the *right thing* for the
customer's actual request. An agent can implement the wrong solution perfectly.

## When to use

After an agent (PM, Dev Lead, QA, etc.) produces a deliverable:
- A PR opened in response to an issue
- A delegation result (A2A `message/send` response)
- A generated config or template
- A code review they posted

**Trigger:** "Agent came back with 'done' — before we believe them."

## What it does

1. Presents the original request and the agent's deliverable to an LLM judge
2. Scores: does the deliverable actually address the request?
3. Reports: passes, partial, or fails — with evidence

## Installation

### In org template (org.yaml)
```yaml
plugins:
  - molecule-skill-llm-judge
```

### From URL
```
github://Molecule-AI/molecule-ai-plugin-molecule-skill-llm-judge
```

## License
Business Source License 1.1 — © Molecule AI.
