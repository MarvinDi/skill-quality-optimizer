# Skill Split Template

This document provides a standard template for splitting an overlong SKILL.md into SKILL.md + references/.

## Table of Contents

- [Before: single-file structure](#before-single-file-structure)
- [After: multi-file structure](#after-multi-file-structure)
- [SKILL.md Template (overview + navigation)](#skillmd-template-overview--navigation)
- [references/ File Template](#references-file-template)
- [Example: 1424-line skill split](#example-1424-line-skill-split)

---

## Before: Single-File Structure

```
SKILL.md  (1424 lines, contains overview, all details, all examples, all rules)
```

At startup AI loads `name`+`description`; when deciding to use it, reads all 1424 lines — attention is diluted.

---

## After: Multi-File Structure

```
SKILL.md                      (341 lines, overview + navigation)
references/
├── test-records.md           (524 lines)
├── image-params.md           (296 lines)
├── cover-text.md             (140 lines)
├── troubleshooting.md         (68 lines)
└── execution-example.md      (55 lines)
```

At startup AI only reads 341 lines; other files read on demand.

---

## SKILL.md Template (overview + navigation)

```markdown
---
name: <skill-name>
description: <one-line function + trigger conditions, under 200 chars>
---

# <Skill Title>

## One-sentence Description
<What the skill does, when to use it>

## File Structure
```
{{SKILL_DIR}}/
├── SKILL.md              ← This file, overview and navigation
└── references/
    ├── <file1>.md        ← <description>
    ├── <file2>.md        ← <description>
    └── ...
```

## Core Capabilities
- <Capability 1>
- <Capability 2>
- <Capability 3>

## Usage Example
<Short example>

## Detailed Rules
See references/ for corresponding rules:
- <Rule A> → references/<fileA>.md
- <Rule B> → references/<fileB>.md

## Quick Self-Check
1. [ ] ...
2. [ ] ...
```

---

## references/ File Template

Each references file follows this structure:

```markdown
# <File Title>

## Table of Contents
- <Section 1>
- <Section 2>
- ...

## <Section 1>
<Content>

## <Section 2>
<Content>

## Code Example
```<lang>
<Example code>
```
```

### File Naming Convention
- Names should reveal content; avoid `doc1.md`, `doc2.md`.
- Use meaningful Chinese or English names, e.g., `test-records.md`, `image-params.md`.

### Line Count Control
- A single file over 100 lines needs a table of contents at the top.
- Suggest each file focuses on one topic, no cross-domain content.

---

## Example: 1424-line skill split

### Before

```
SKILL.md: 1424 lines
```

### After

| File | Lines | Description |
|------|-------|-------------|
| SKILL.md | 341 | Overview, capabilities, usage example, quick self-check, references navigation |
| references/test-records.md | 524 | Test data and records for each module |
| references/image-params.md | 296 | Image generation parameter descriptions |
| references/cover-text.md | 140 | Cover text overlay rules |
| references/troubleshooting.md | 68 | Common problem troubleshooting |
| references/execution-example.md | 55 | Execution example code |

### Improvement Effect
- AI loads only 341 lines at startup (down 76%).
- When editing, no need to search keywords first; scan 341 lines from top to locate.
- Each references file focuses on a single topic, easy to maintain.