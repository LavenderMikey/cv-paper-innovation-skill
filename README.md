# cv-paper-innovation

一个面向计算机视觉科研用户的 Agent Skill，可同时用于 Claude Code / Claude Agent 和 Codex：读入多篇 CV 论文，挖掘可落地、能解决真实问题、改进后有希望涨点的创新框架，并做 novelty check，避免重复造轮子。

## 解决什么问题

科研用户面对一摞论文时，真实诉求通常不是"总结论文"，而是找到一个能写成论文、能跑实验、能解释为什么可能涨点的新方案。本 Skill 把这件事拆成可复用工作流，默认输出结构化中文 Markdown 报告。

它始终带着四条自检标准：

1. 可落地、能实现：具体到模块、机制、接口或实验设置。
2. 解决真实存在的问题：挂钩到论文里的局限、失败场景或未覆盖任务。
3. 改进后有涨点逻辑：说明机制原因和可验证实验。
4. 尽量不撞车：检查相似工作，给出风险等级和差异化建议。

## 触发方式

把 CV 论文 PDF、论文笔记或论文列表交给 Claude 或 Codex，并表达类似意图即可：

- "帮我从这几篇论文里找创新点"
- "这几篇论文能不能想个新点子"
- "怎么改进能涨点"
- "把这几篇论文的模块结合一下"
- "帮我设计一个可以写论文的新模型"

## 工作流程

| 步骤 | 内容 |
| --- | --- |
| 1. 确定阅读方案 | 按论文数量自适应：少量精读，多篇先筛后读 |
| 2. 加载领域参考 | 按检测、分割、跟踪等方向只加载必要 reference |
| 3. 逐篇解析 | 提取任务、方法、创新点、指标、局限 |
| 4. 检索相关工作 | 需要最新性或撞车检查时联网核实 |
| 5. 形成新模型 | 给出 baseline + 2-3 条具体改动 |
| 6. novelty check | 判断 primitive 和本子领域是否已被做过 |
| 7. 推荐排序 | 明确强推、谨慎、不建议优先做的方向 |

## 仓库结构

```text
cv-paper-innovation/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── common.md
    ├── detection.md
    ├── segmentation.md
    └── tracking.md
```

`references/` 提供各方向的 baseline、数据集、评价指标和常见痛点。具体数值只作量级参考，涉及精确指标时仍需核实原论文或最新 leaderboard。

## 安装与使用

本仓库已做双端适配：

- Claude 使用 `SKILL.md` 和 `references/` 中的工作流与领域参考。
- Codex 使用同一份 `SKILL.md`，并额外读取 `agents/openai.yaml` 作为界面元数据和默认提示。

### Claude Code / Claude Agent

用户级安装：

```bash
mkdir -p ~/.claude/skills
cp -R cv-paper-innovation ~/.claude/skills/
```

Windows PowerShell 示例：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills" | Out-Null
Copy-Item -Recurse -Force .\cv-paper-innovation "$env:USERPROFILE\.claude\skills\"
```

项目级安装：

```bash
mkdir -p .claude/skills
cp -R cv-paper-innovation .claude/skills/
```

安装后重启或重新加载 Claude Code / Claude Agent。之后可以直接提出"帮我从这几篇论文里找创新点"这类请求，也可以显式说明使用 `cv-paper-innovation` skill。

### Codex

用户级安装：

```bash
mkdir -p ~/.codex/skills
cp -R cv-paper-innovation ~/.codex/skills/
```

Windows PowerShell 示例：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills" | Out-Null
Copy-Item -Recurse -Force .\cv-paper-innovation "$env:USERPROFILE\.codex\skills\"
```

项目级安装：

```bash
mkdir -p .codex/skills
cp -R cv-paper-innovation .codex/skills/
```

安装后重启或重新加载 Codex，即可通过 `$cv-paper-innovation` 显式调用，或在匹配相关科研意图时被自动触发。

## 输出骨架

```markdown
## 阅读范围

## 推荐模型：[名字]（暂名）
**Baseline：** ...

**核心改动**
1. ...
2. ...
3. ...

**为什么可能涨点：** ...

**实验设计：**
- Baseline：
- 数据集 / 指标：
- 消融：
- 成本统计：

## 撞车检查
| 改动 | 风险 | 最接近工作 | 差异化建议 |

## 推荐程度
| 程度 | 方向 | 理由 |
```

## 设计理念

两三个扎实、能落地、能验证的提案，远好过十个听起来很炫但没法实现的点子。未联网时，凡涉及具体论文名、作者、年份、数值或链接，都应标注需要人工核实。
