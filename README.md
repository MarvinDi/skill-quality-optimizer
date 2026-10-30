# skill-quality-optimizer

**English** | [简体中文](./README.zh-CN.md)

An agent skill that analyzes, lints, and refactors SKILL.md files.

## Install

### Via skills.sh

```bash
npx skills add MarvinDi/skill-quality-optimizer
```

### Manual Install

```bash
git clone https://github.com/MarvinDi/skill-quality-optimizer.git
```

## When to Use

Invoke this skill when a SKILL.md has any of these issues:

1. Over 500 lines — AI can't locate rules
2. `description` only says "what I do", not "when to call me"
3. Body has requirements but no code output examples
4. Same rule duplicated across skill, project rules, and system prompt
5. Never tested with multiple phrasings for trigger stability

This skill outputs a quality report, split plan, description rewrite suggestions, code examples, and multi-phrasing test cases.

## Structure

```
skill-quality-optimizer/
├── SKILL.md              ← Main skill definition (<500 lines, with trigger conditions)
├── README.md
├── README.zh-CN.md
├── LICENSE
├── references/
│   ├── skill-lint-rules.md   ← Detailed check rules for the five pitfalls
│   ├── split-template.md     ← Skill split template
│   └── test-phrasings.md     ← Multi-phrasing test case template
└── examples/
    ├── before/
    │   └── overlong-skill/       ← Overlong skill example (before optimization)
    └── after/
        └── skill-quality-optimizer/  ← Split skill example (after optimization)
            ├── SKILL.md
            └── references/
```

## Principle

Based on the article [《Skill 写太长，AI 反而记不住》](https://mp.weixin.qq.com/s/B6hbLin681pqZg5Fzz2ClQ), originally published (in Chinese) on the WeChat Official Account "大模型AI之旅" on 2026-10-29:

- AI reads skills in three layers: at startup it only loads `name` + `description`; it reads the body only when it decides to use the skill; it opens `references/` files only when truly needed.
- Every line in the body competes with conversation history for context window space. Keep the SKILL.md body under 500 lines.

This skill practices what it preaches: SKILL.md body < 500 lines, `description` includes trigger conditions, details live in `references/`.

## License

[MIT](LICENSE)
