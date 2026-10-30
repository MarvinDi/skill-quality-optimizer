---
name: skill-quality-optimizer
description: "Analyzes, lints, and refactors SKILL.md files. Invoked when a skill is too long, lacks trigger conditions in description, has no code examples, or duplicates rules across files. Splits overlong skills into SKILL.md + references/."
---

# Skill Quality Optimizer (After Split Example)

This skill analyzes, optimizes, and splits existing SKILL.md files to follow the best practices from "Skill is too long, AI can't remember".

## File Structure

```
{{SKILL_DIR}}/
├── SKILL.md              ← This file, overview and navigation (341 lines)
└── references/
    ├── skill-lint-rules.md   ← Five pitfalls detailed check rules
    ├── split-template.md     ← Skill split template
    └── test-phrasings.md     ← Multi-phrasing test case template
```

## Core Capabilities

1. **Quality lint**: Scan SKILL.md, locate five common problems.
2. **Split recommendation**: For skills over 500 lines, propose a split plan.
3. **Description rewrite**: Add "when to invoke" trigger conditions.
4. **Code example generation**: Add code block examples for skills missing output samples.
5. **Deduplication check**: Compare against project rules and system prompt, flag duplicated rules.
6. **Multi-phrasing testing**: Generate 3 different phrasing test cases to verify skill trigger stability.

## Five Pitfalls Self-Check Checklist

| Pitfall | Problem | Check Method |
|---------|---------|-------------|
| Pitfall 1 | description missing trigger conditions | Read only description; can someone unfamiliar say when to use? |
| Pitfall 2 | One skill does too much, not split | Line count > 500 means split |
| Pitfall 3 | No code examples | Body has at least one code block |
| Pitfall 4 | Rules duplicated across files | Skill/project rules/system prompt each define same thing only once |
| Pitfall 5 | Not multi-phrasing tested | Same requirement with 3 phrasings all trigger same skill |

## Detailed Rules

- Five pitfalls details → `references/skill-lint-rules.md`
- Split template → `references/split-template.md`
- Test cases → `references/test-phrasings.md`

## Output Example

```
## Quality Report: <skill-name>

### Line Count
- Current: 1424 lines
- Recommended: <500 lines

### Split File Structure
- SKILL.md: 341 lines
- references/test-records.md: 524 lines
- references/image-params.md: 296 lines
- references/cover-text.md: 140 lines
- references/troubleshooting.md: 68 lines
- references/execution-example.md: 55 lines
```

## Quick Self-Check

1. [ ] description includes "when to invoke" trigger conditions?
2. [ ] Line count under 500?
3. [ ] Body has at least one code block output example?
4. [ ] No duplication with project rules, system prompt?
5. [ ] Same requirement tested with 3 phrasings, all trigger same skill?