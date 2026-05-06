# Local Development Setup

This runbook covers setting up a local development environment for
`molecule-skill-llm-judge`.

---

## Prerequisites

- Python 3.11+
- `gh` CLI authenticated
- Write access to `Molecule-AI/molecule-ai-plugin-molecule-skill-llm-judge`

---

## Clone & Bootstrap

```bash
git clone https://github.com/Molecule-AI/molecule-ai-plugin-molecule-skill-llm-judge.git
cd molecule-ai-plugin-molecule-skill-llm-judge
```

---

## Validating Plugin Structure

```bash
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"
echo "plugin.yaml OK"
```

---

## Testing the LLM Judge

The harness wrapper is provided by the Molecule AI platform at runtime.
To test:

1. Install the plugin in a test workspace
2. Create a test issue with a clear request
3. Submit a deliberately wrong deliverable
4. Run `llm-judge` and verify the score is low (below threshold)

Example:
```
Request: "Add user authentication with JWT tokens"
Deliverable: "Added logging to all API endpoints"
Expected score: 1-2
```

---

## Tuning the Judge Prompt

If the judge is consistently wrong, adjust the scoring criteria in
`skills/llm-judge/SKILL.md`. Key things to tune:
- Clarity of the original request
- Whether the deliverable was checked against the request
- Calibration of score 3 vs score 4

---

## Troubleshooting

### Judge always scores 5

- The judge prompt may be too lenient
- Verify the original request is included in the judge prompt

### Judge scores 1 on good work

- The judge prompt may be too strict
- Check the criteria — ensure "correct but different approach" scores ≥ 4

### Inconsistent scores between runs

- LLM judges have inherent non-determinism
- Consider adding a temperature of 0 to reduce variance

---

## Related

- `skills/llm-judge/SKILL.md` — scoring criteria and usage
