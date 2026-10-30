---
name: skill-quality-optimizer
description: "Analyzes, lints, and refactors SKILL.md files. Invoked when a skill is too long, lacks trigger conditions in description, has no code examples, or duplicates rules across files. Splits overlong skills into SKILL.md + references/."
license: MIT
metadata:
  version: "1.1.0"
---

# Skill Quality Optimizer

This skill analyzes, optimizes, and splits existing SKILL.md files to follow the best practices from the article "Skill is too long, AI can't remember":

- AI reads skills in three layers: at startup it only loads `name`+`description`; it reads the body only when it decides to use the skill; it opens `references/` files only when truly needed.
- Every line in the body competes with conversation history for context window space. Keep SKILL.md body under 500 lines.

## Core Capabilities

1. **Quality lint**: Scan SKILL.md to locate five common problems (see "Five Pitfalls" below).
2. **Split recommendation**: For skills over 500 lines, propose a split: SKILL.md keeps overview+navigation, details move to `references/`.
3. **Description rewrite**: Add "when to invoke" trigger conditions to make description self-contained.
4. **Code example generation**: Add code block examples for skills missing output samples.
5. **Deduplication check**: Compare against project rules and system prompts, flag duplicated rule definitions.
6. **Multi-phrasing testing**: Generate 3 different phrasing test cases to verify skill trigger stability.

## Five Pitfalls and Self-Check Checklist

| Pitfall | Problem | Check Method | Fix |
|---------|---------|-------------|-----|
| Pitfall 1 | `description` only says "what I do", not "when to call me" | Read only description; ask someone unfamiliar with the project "when should this be used?" — if they can't answer, rewrite | Add trigger conditions to description, e.g., "Use when user mentions 'financial report', 'quarterly report', 'extract financial data'" |
| Pitfall 2 | One skill does three or four things; AI searches thousands of lines for the few dozen lines it needs now | Count lines. Over 500 lines means split first | SKILL.md keeps only overview and navigation; details go to `references/`, loaded on demand |
| Pitfall 3 | Only requirements, no examples | Scan body; if no code block, add one | Add a code block output example — examples set the standard for field names, granularity, indentation |
| Pitfall 4 | Same thing written in skill, project rules, and system prompt | Lay all three side by side; check if the same thing is defined twice | Define ownership first, then remove duplicates elsewhere |
| Pitfall 5 | Published without testing | Test the same requirement with three different phrasings; check if AI always takes the same path | If one deviates, a rule is being bypassed or two rules conflict |

## `references/` Usage Rules

- References are one layer deep: SKILL.md can only directly reference files in `references/`, no nested references.
- A single reference file over 100 lines needs a table of contents at the top.
- File names should reveal content; avoid `doc1.md`, `doc2.md`.
- Body keeps only overview and navigation; `references/` files are read on demand.

## Output Format

Optimization suggestions follow this structure:

```
## Quality Report: <skill-name>

### Line Count
- Current: <N> lines
- Recommended: <500 lines

### Issues
- Pitfall 1: <description missing trigger> → <suggestion>
- Pitfall 2: <too long, not split> → <split plan>
- Pitfall 3: <no code example> → <example code>
- Pitfall 4: <duplicated with project rules> → <dedup suggestion>
- Pitfall 5: <not multi-phrasing tested> → <test cases>

### Split File Structure (if applicable)
- SKILL.md: <N> lines (overview + navigation)
- references/<file1>.md: <N> lines
- references/<file2>.md: <N> lines
```

## Example

User says: "My skill has 1424 lines, AI can't remember the rules."

This skill responds:

1. Read the SKILL.md, count lines (1424 > 500, triggers Pitfall 2).
2. Check if description contains trigger conditions (triggers Pitfall 1).
3. Check if there are code block examples (triggers Pitfall 3).
4. Output quality report and split plan:
   - SKILL.md: 341 lines (overview)
   - references/test-records.md: 524 lines
   - references/image-params.md: 296 lines
   - references/cover-text.md: 140 lines
   - references/troubleshooting.md: 68 lines
   - references/execution-example.md: 55 lines
5. Generate 3 phrasing test cases to verify trigger stability after split.

## What This Skill Does NOT Do

- Does not directly modify remote repositories; outputs suggestions and copyable file content only.
- Does not force splitting; if under 500 lines with no other issues, recommends keeping as-is.