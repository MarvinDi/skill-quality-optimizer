# skill-quality-optimizer

技能质量优化助手 / Skill Quality Optimizer — Analyzes, lints, and refactors SKILL.md files.

---

## English

### Install

#### Via skills.sh

```bash
npx skills add MarvinDi/skill-quality-optimizer
```

#### Manual Install

Clone this repository:

```bash
git clone https://github.com/MarvinDi/skill-quality-optimizer.git
```

### Usage

Invoke this skill when your SKILL.md has any of these issues:

1. Over 500 lines, AI can't locate rules
2. `description` only says "what I do", not "when to call me"
3. Body has requirements but no code output examples
4. Same rule duplicated across skill, project rules, and system prompt
5. Not tested with multiple phrasings for trigger stability

This skill outputs a quality report, split plan, description rewrite suggestions, code examples, and multi-phrasing test cases.

### Structure

```
skill-quality-optimizer/
├── SKILL.md              ← Main skill definition (<500 lines, with trigger conditions)
├── README.md
├── LICENSE
├── references/
│   ├── skill-lint-rules.md   ← Five pitfalls detailed check rules
│   ├── split-template.md     ← Skill split template
│   └── test-phrasings.md     ← Multi-phrasing test case template
└── examples/
    ├── before/
    │   └── overlong-skill/       ← Optimization-before overlong skill example
    └── after/
        └── skill-quality-optimizer/  ← Optimization-after split skill example
            ├── SKILL.md
            └── references/
```

### Principle

Based on the article "Skill is too long, AI can't remember":

- AI reads skills in three layers: at startup it only loads `name`+`description`; it reads the body only when it decides to use the skill; it opens `references/` files only when truly needed.
- Every line in the body competes with conversation history for context window space. Keep SKILL.md body under 500 lines.

This skill practices what it preaches: SKILL.md body < 500 lines, `description` includes trigger conditions, details in `references/`.

### License

[MIT](LICENSE)

---

## 中文

### 安装

#### 通过 skill.sh

```bash
npx skills add MarvinDi/skill-quality-optimizer
```

#### 手动安装

克隆本仓库：

```bash
git clone https://github.com/MarvinDi/skill-quality-optimizer.git
```

### 用途

当你的 SKILL.md 出现以下情况时，调用本 skill：

1. 文件超过 500 行，AI 难以定位规则
2. `description` 只写“我会干什么”，没写“什么时候找我”
3. 正文只有要求，没有代码输出示例
4. 同一规则在技能、项目规则、系统提示中重复出现
5. 写完未用多种说法测试触发稳定性

本 skill 会输出质量报告、拆分方案、description 重写建议、代码示例和多 phrasing 测试用例。

### 目录结构

```
skill-quality-optimizer/
├── SKILL.md              ← 主 skill 定义（<500行，含触发条件）
├── README.md
├── LICENSE
├── references/
│   ├── skill-lint-rules.md   ← 五个坑的详细检查规则
│   ├── split-template.md     ← 技能拆分模板
│   └── test-phrasings.md     ← 多 phrasing 测试用例模板
└── examples/
    ├── before/
    │   └── overlong-skill/       ← 优化前的过长技能示例
    └── after/
        └── skill-quality-optimizer/  ← 拆分后的示例
            ├── SKILL.md
            └── references/
```

### 原理

参考文章《Skill 写太长，AI 反而记不住》：

- AI 读技能分三层：启动时只读 `name`+`description`；判定要用才读正文；真需要才打开 `references/`。
- 写进正文的每一行都在跟对话历史抢上下文位置，建议 SKILL.md 正文控制在 500 行以内。

本 skill 本身也以身作则：SKILL.md 正文 < 500 行，`description` 包含触发条件，细节文件放在 `references/`。

### 许可证

[MIT](LICENSE)