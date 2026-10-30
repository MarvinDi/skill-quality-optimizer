# skill-quality-optimizer

[English](./README.md) | **简体中文**

一个用于分析、检查和重构 SKILL.md 文件的 Agent 技能。

## 安装

### 通过 skills.sh

```bash
npx skills add MarvinDi/skill-quality-optimizer
```

### 手动安装

```bash
git clone https://github.com/MarvinDi/skill-quality-optimizer.git
```

## 何时使用

当 SKILL.md 出现以下情况时，调用本技能：

1. 文件超过 500 行，AI 难以定位规则
2. `description` 只写“我会干什么”，没写“什么时候找我”
3. 正文只有要求，没有代码输出示例
4. 同一规则在技能、项目规则、系统提示中重复出现
5. 写完未用多种说法测试触发稳定性

本技能会输出质量报告、拆分方案、description 重写建议、代码示例和多 phrasing 测试用例。

## 目录结构

```
skill-quality-optimizer/
├── SKILL.md              ← 主 skill 定义（<500行，含触发条件）
├── README.md
├── README.zh-CN.md
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

## 原理

参考文章《Skill 写太长，AI 反而记不住》：

- AI 读技能分三层：启动时只读 `name` + `description`；判定要用才读正文；真需要才打开 `references/`。
- 写进正文的每一行都在跟对话历史抢上下文位置，建议 SKILL.md 正文控制在 500 行以内。

本技能本身也以身作则：SKILL.md 正文 < 500 行，`description` 包含触发条件，细节文件放在 `references/`。

## 许可证

[MIT](LICENSE)
