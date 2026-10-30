---
name: skill-quality-optimizer
description: "Analyzes, lints, and refactors SKILL.md files. Invoked when a skill is too long, lacks trigger conditions in description, has no code examples, or duplicates rules across files. Splits overlong skills into SKILL.md + references/."
---

# Skill Quality Optimizer（拆分后示例）

本 skill 用于分析、优化和拆分现有 SKILL.md，使其符合“大模型AI之旅”文章《Skill 写太长，AI 反而记不住》提出的最佳实践。

## 文件结构
```
{{SKILL_DIR}}/
├── SKILL.md              ← 本文件，概览和导航（341 行）
└── references/
    ├── skill-lint-rules.md   ← 五个坑的详细检查规则
    ├── split-template.md     ← 技能拆分模板
    └── test-phrasings.md     ← 多 phrasing 测试用例模板
```

## 核心能力
1. **质量检查（lint）**：扫描 SKILL.md，定位五个常见问题。
2. **拆分建议**：对超过 500 行的技能，给出拆分方案。
3. **description 重写**：补全“何时调用”的触发条件。
4. **示例代码生成**：为缺少输出示例的技能补充代码块。
5. **去重检查**：与项目规则、系统提示对比，标记重复规则。
6. **多 phrasing 测试**：生成 3 种不同说法的测试用例。

## 五个坑自查清单

| 坑 | 问题 | 检查方法 |
|----|------|---------|
| 坑一 | description 缺触发条件 | 只看 description，不熟悉项目的人能答出什么时候用吗？ |
| 坑二 | 一技多事未拆分 | 行数 > 500 则拆分 |
| 坑三 | 无代码示例 | 正文至少有一个代码块 |
| 坑四 | 多处规则重复 | 技能/项目规则/系统提示中同一事只定义一次 |
| 坑五 | 未多 phrasing 测试 | 同一需求换三种说法，都触发同一技能 |

## 详细规则
- 五个坑详解 → `references/skill-lint-rules.md`
- 拆分模板 → `references/split-template.md`
- 测试用例 → `references/test-phrasings.md`

## 输出示例

```
## 质量报告：<技能名>

### 行数统计
- 当前：1424 行
- 建议：<500 行

### 拆分后文件结构
- SKILL.md：341 行
- references/实测记录.md：524 行
- references/生图参数.md：296 行
- references/封面叠字.md：140 行
- references/排障.md：68 行
- references/执行示例.md：55 行
```

## 快速自查
1. [ ] description 包含“何时调用”的触发条件？
2. [ ] 行数 < 500？
3. [ ] 正文至少有一个代码块输出示例？
4. [ ] 与项目规则、系统提示无重复？
5. [ ] 同一需求换三种说法测试，都触发同一技能？