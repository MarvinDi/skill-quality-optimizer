# Skill Lint Rules

This document details the five pitfalls from "Skill is too long, AI can't remember" and their check/fix rules.

## Table of Contents

- [Pitfall 1: description missing trigger conditions](#pitfall-1-description-missing-trigger-conditions)
- [Pitfall 2: one skill does too much, not split](#pitfall-2-one-skill-does-too-much-not-split)
- [Pitfall 3: only requirements, no examples](#pitfall-3-only-requirements-no-examples)
- [Pitfall 4: rules duplicated across files](#pitfall-4-rules-duplicated-across-files)
- [Pitfall 5: not multi-phrasing tested](#pitfall-5-not-multi-phrasing-tested)

---

## Pitfall 1: description missing trigger conditions

### Problem
`description` only says "what I do", not "when to call me". At startup AI only loads `name`+`description`. If description doesn't include trigger scenarios, AI won't know when to invoke this skill.

### Check Method
Read only `description`; ask someone unfamiliar with the project "when should this be used?" — if they can't answer, rewrite.

### Fix Example

**❌ Before**:
```
description: "Extract revenue, net profit, and growth rate from PDF financial reports, output JSON."
```

**✅ After**:
```
description: "Extract revenue, net profit, and growth rate from PDF quarterly financial reports, output JSON. Use when user mentions 'financial report', 'quarterly report', 'extract financial data'."
```

### Verification Standard
`description` must include both:
1. What the skill does (functionality)
2. When to invoke it (trigger conditions/keywords/scenarios)

---

## Pitfall 2: one skill does too much, not split

### Problem
One skill does three or four things; AI searches thousands of lines for the few dozen lines it needs now. The longer the body, the thinner the attention for the current task.

### Check Method
Count lines. Over 500 lines means split first.

### Fix Method
Split into three-layer structure:

```
SKILL.md          ← Overview + navigation (<500 lines)
references/       ← Details, loaded on demand
  ├── <file1>.md  ← Single file over 100 lines needs a table of contents at top
  ├── <file2>.md  ← File names should reveal content, avoid doc1.md/doc2.md
  └── <file3>.md
```

### Companion Rules
1. References are one layer deep: SKILL.md can only directly reference files in `references/`, no nested references.
2. A single reference file over 100 lines needs a table of contents at the top.
3. File names should reveal content; avoid `doc1.md`, `doc2.md`.

---

## Pitfall 3: only requirements, no examples

### Problem
Body only has requirements, no code block output examples. When AI sees an example, it copies field names, granularity, indentation. Examples set the standard, they are not explanations.

### Check Method
Scan the body; if no code block, add one — add the simplest output example.

### Fix Example

Add at the end of SKILL.md:

```markdown
## Output Example

```json
{
  "revenue": 1000000,
  "net_profit": 200000,
  "growth_rate": 0.15
}
```
```

### Verification Standard
Body must include at least one code block showing the skill's typical output format.

---

## Pitfall 4: rules duplicated across files

### Problem
The same thing is written in the skill, project rules files, and system prompt. The undeleted copy stays silent until the remaining copy is changed, then it takes effect — causing conflicts.

### Check Method
Lay the skill, project rules, and system prompt side by side; check if the same thing is defined twice.

### Fix Method
1. Define ownership first (which file is the source of truth).
2. Remove duplicates from elsewhere.
3. Keep one source of truth, reference or delete from others.

---

## Pitfall 5: not multi-phrasing tested

### Problem
Published without testing. Test the same requirement with three different phrasings; AI might take different paths, meaning a rule is being bypassed or two rules conflict.

### Check Method
Test the same requirement with three different phrasings; check if AI always takes the same path.

### Test Case Template

See `test-phrasings.md`.

---

## Quick Self-Check Checklist

1. [ ] Read only description; can someone unfamiliar with the project say when to use it?
2. [ ] Line count under 500? Over 500 means split.
3. [ ] Body has at least one code block output example?
4. [ ] Skill, project rules, system prompt each define the same thing only once?
5. [ ] Same requirement tested with three phrasings, all trigger the same path?