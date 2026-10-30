# skill-quality-optimizer

技能质量优化助手——用于分析、优化和拆分现有 SKILL.md，使其符合《Skill 写太长，AI 反而记不住》提出的最佳实践。

## 安装

### 通过 skill.sh

```bash
npx skills add https://github.com/<你的GitHub用户名>/skill-quality-optimizer
```

### 手动安装

将本仓库克隆到你的技能目录：

```bash
git clone https://github.com/<你的GitHub用户名>/skill-quality-optimizer.git .trae/skills/skill-quality-optimizer
```

## 用途

当你的 SKILL.md 出现以下情况时，调用本 skill：

1. 文件超过 500 行，AI 难以定位规则
2. `description` 只写“我会干什么”，没写“什么时候找我”
3. 正文只有要求，没有代码输出示例
4. 同一规则在技能、项目规则、系统提示中重复出现
5. 写完未用多种说法测试触发稳定性

本 skill 会输出质量报告、拆分方案、description 重写建议、代码示例和多 phrasing 测试用例。

## 目录结构

```
skill-quality-optimizer/
├── SKILL.md              ← 主 skill 文件（<500行，含触发条件）
├── references/
│   ├── skill-lint-rules.md   ← 五个坑的详细检查规则
│   ├── split-template.md     ← 技能拆分模板
│   └── test-phrasings.md     ← 多 phrasing 测试用例模板
├── examples/
│   ├── before/
│   │   └── overlong-skill/   ← 优化前的过长技能示例
│   └── after/
│       └── skill-quality-optimizer/  ← 拆分后的示例
├── README.md
└── LICENSE
```

## 原理

参考文章《Skill 写太长，AI 反而记不住》：

- AI 读技能分三层：启动时只读 `name`+`description`；判定要用才读正文；真需要才打开 `references/`。
- 写进正文的每一行都在跟对话历史抢上下文位置，建议 SKILL.md 正文控制在 500 行以内。

本 skill 本身也以身作则：SKILL.md 正文 < 500 行，`description` 包含触发条件，细节文件放在 `references/`。

## 许可证

[MIT](LICENSE)