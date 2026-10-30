---
name: skill-quality-optimizer
description: "Analyzes, lints, and refactors SKILL.md files. Invoked when a skill is too long, lacks trigger conditions in description, has no code examples, or duplicates rules across files. Splits overlong skills into SKILL.md + references/."
---

# Skill Quality Optimizer

本 skill 用于分析、优化和拆分现有 SKILL.md，使其符合“大模型AI之旅”文章《Skill 写太长，AI 反而记不住》提出的最佳实践：

- AI 读技能分三层：启动时只读 `name`+`description`；判定要用才读正文；真需要才打开 `references/`。
- 写进正文的每一行都在跟对话历史抢上下文位置，建议 SKILL.md 正文控制在 500 行以内。

## 核心能力

1. **质量检查（lint）**：扫描 SKILL.md，定位五个常见问题（见下方“五个坑”）。
2. **拆分建议**：对超过 500 行的技能，给出拆分方案：SKILL.md 保留概览+导航，细节移至 `references/`。
3. **description 重写**：补全“何时调用”的触发条件，使 description 自足。
4. **示例代码生成**：为缺少输出示例的技能补充代码块示例。
5. **去重检查**：与项目规则、系统提示对比，标记重复定义的规则。
6. **多 phrasing 测试**：生成 3 种不同说法的测试用例，验证技能触发稳定性。

## 五个坑与自查清单

| 坑 | 问题 | 检查方法 | 改法 |
|----|------|---------|------|
| 坑一 | `description` 只写“我会干什么”，没写“什么时候找我” | 只看 description，问不熟悉项目的人“什么时候该用”，答不上来就重写 | 把触发条件写进 description，例如：“用户提到‘财报’‘季报’‘提取财务数据’时使用” |
| 坑二 | 一份技能干三件事（甚至四件），AI 每次都在几千行里找此刻需要的几十行 | 数行数，>500 行先拆分 | SKILL.md 只留概览和导航，细节放 `references/`，用到才读 |
| 坑三 | 只写要求，不写样例 | 翻一遍正文，无代码块就加 | 贴代码块输出示例，定标字段名、粒度、缩进 |
| 坑四 | 同一件事在技能、项目规则、系统提示里各写一遍 | 把三者摊在一起，看有没有对同一事说了两种话 | 先定义归谁管，再从别处删掉重复 |
| 坑五 | 写完就发，没跑过 | 同一个需求换三种说法测试，看 AI 是否都走到同一条路 | 有一种跑偏，说明有条规则被绕过去，或两处规则在打架 |

## references/ 使用规则

- 引用只能一层：SKILL.md 只能直接引用 `references/` 下的文件，不能再嵌套引用。
- 单个参考文件超过 100 行，顶部加目录。
- 文件名要能看出内容，避免 `doc1.md`、`doc2.md`。
- 正文只留概览和导航，`references/` 里的文件用到才读。

## 输出格式

优化建议按以下结构输出：

```
## 质量报告：<技能名>

### 行数统计
- 当前：<N> 行
- 建议：<500 行

### 问题清单
- 坑一：<description 缺触发条件> → <建议>
- 坑二：<过长未拆分> → <拆分方案>
- 坑三：<无代码示例> → <示例代码>
- 坑四：<与项目规则重复> → <去重建议>
- 坑五：<未多 phrasing 测试> → <测试用例>

### 拆分后文件结构（如适用）
- SKILL.md：<行数> 行（概览+导航）
- references/<文件1>.md：<行数> 行
- references/<文件2>.md：<行数> 行
```

## 使用示例

用户说：“我的 skill 有 1424 行，AI 记不住规则。”  
本 skill 响应：

1. 读取该 SKILL.md，统计行数（1424 > 500，触发坑二）。
2. 检查 description 是否含触发条件（触发坑一）。
3. 检查是否有代码块示例（触发坑三）。
4. 输出质量报告和拆分方案：
   - SKILL.md：341 行（概览）
   - references/实测记录.md：524 行
   - references/生图参数.md：296 行
   - references/封面叠字.md：140 行
   - references/排障.md：68 行
   - references/执行示例.md：55 行
5. 生成 3 种 phrasing 的测试用例，验证拆分后触发稳定性。

## 不做什么

- 不替用户直接修改远程仓库，只输出建议和可复制的文件内容。
- 不强制拆分，行数 < 500 且无其他问题时建议保持原样。