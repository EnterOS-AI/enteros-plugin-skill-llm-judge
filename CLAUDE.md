# molecule-skill-llm-judge — LLM-as-Judge Gate

`molecule-skill-llm-judge` is a **cheap LLM-as-judge gate** that scores whether
a deliverable (PR diff, A2A response, generated config) actually addresses the
original request. It catches the failure mode unit tests miss: the code works
but solves the wrong problem.

**Version:** 1.0.0
**Runtime:** `claude_code`

---

## Repository Layout

```
molecule-skill-llm-judge/
├── plugin.yaml              — Plugin manifest
├── skills/
│   └── llm-judge/
│       └── SKILL.md         — Scoring criteria and process
└── adapters/               — Harness adaptors
```

---

## How It Works

### The Judge Prompt

The skill sends the original request + the deliverable to a judge LLM and
asks for a score 1–5:

| Score | Meaning |
|---|---|
| 5 | Deliverable fully addresses the request |
| 4 | Addresses most of the request, minor gaps |
| 3 | Partial address, significant gaps |
| 2 | Mostly irrelevant |
| 1 | Completely wrong |

### Gate Behaviour

Configure the threshold in workspace settings:

```json
{
  "llm_judge": {
    "threshold": 4,
    "model": "claude-sonnet-4-20250514"
  }
}
```

If the score is below threshold, the skill returns a denial with the judge's reasoning.

---

## When to Use

✅ Use for:
- Verifying PR diffs against the original issue
- Checking A2A responses address the task
- Validating generated configs against requirements

❌ Don't use for:
- Well-tested pure logic (unit tests catch this)
- Exploratory work where "wrong" isn't well-defined

---

## Development

### Prerequisites

- Python 3.11+
- `gh` CLI authenticated
- Write access to `Molecule-AI/molecule-ai-plugin-molecule-skill-llm-judge`

### Setup

```bash
git clone https://github.com/Molecule-AI/molecule-ai-plugin-molecule-skill-llm-judge.git
cd molecule-ai-plugin-molecule-skill-llm-judge
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"
```

### Pre-Commit Checklist

```bash
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"

python3 -c "
import re, sys
with open('plugin.yaml') as f:
    content = f.read()
patterns = [r'sk.ant', r'ghp.', r'AKIA[A-Z0-9]']
if any(re.search(p, content) for p in patterns):
    print('FAIL: possible credentials found')
    sys.exit(1)
print('No credentials: OK')
"
```

---

## Release Process

1. Review changes: `git log origin/main..HEAD --oneline`
2. Bump `version` in `plugin.yaml` (semver)
3. Commit: `chore: bump version to X.Y.Z`
4. Tag and push: `git tag vX.Y.Z && git push origin main --tags`
5. Create GitHub Release with changelog

---

## Known Issues

See `known-issues.md` at the repo root.
