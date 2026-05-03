# Document Governance Skill

[English](README.md) | [简体中文](README.zh-CN.md)

一个可复用的 AI Agent Skill，用来把项目文档整理成清晰、低上下文成本、可长期维护的结构。

想象一个很常见的场景：你让 Agent 接手一个已有项目，结果它一打开 `docs/` 就看到一大堆 Markdown，里面混着旧 Sprint 计划、过期 TODO、历史决策、API 说明、部署命令、研究笔记和原始记录。Agent 分不清什么是当前事实，什么只是历史记录，于是每次开工都浪费上下文，甚至被旧文档带偏。

这个 skill 就是给 Agent 一套通俗、可执行的文档整理方法。

## 它解决什么问题

长期迭代的项目很容易积累出一堆“看似有用但很难使用”的文档：

- 一个文件里同时混着当前状态、旧计划、历史决策、部署步骤和原始调研
- AI Agent 每次开工都浪费上下文读取过时 Sprint 日志或已废弃决策
- specs、ADR、runbooks、研究笔记互相漂移，不知道哪个才是当前事实
- 不清楚一次改动应该更新 spec、ADR、progress 还是 runbook
- 大量截图、导出文件、研究材料污染日常开发上下文
- 高风险代码的红线规则存在，但要么每次都被过度读取，要么真正需要时被漏掉

这个 skill 的目标是：按职责拆分文档，定义读取顺序，让高价值上下文可发现，但不让每次会话都背负整个项目历史。

## 适用场景

适合在这些情况下使用：

- 仓库里的 `docs/` 目录已经变大、过时、难导航
- 你希望为未来的 AI 编码会话设计一条低 token 的读取路径
- 你需要区分当前状态、specs、ADR、runbooks、research、archive
- 历史计划或旧决策正在误导新成员或 AI Agent
- 你想明确什么时候应该更新 spec、ADR 或 progress
- 你正在移动文档文件，需要同步修复引用路径
- 你需要为高风险代码建立 guardrail / locked-file 文档规则
- 你准备把项目交接给另一个工程师或 AI Agent

它尤其适合 AI 辅助开发项目，因为这类项目非常依赖上下文质量；过期文档很容易把 Agent 带到错误方向。

## 不适用场景

以下情况通常不需要这个 skill：

- 项目很小，只有一两个短文档
- 只是想润色 README 文案
- 要生成面向最终用户的产品文档
- 想用通用模板替代真正的技术 spec 或 ADR 内容

## 能力范围

你可以让任意 AI Agent 用它来：

- 清理混乱的 `docs/` 目录
- 定义每类文档的职责边界
- 拆分当前状态、specs、ADR、runbooks、research 和 archive
- 降低历史文档带来的上下文负担
- 移动文档后修复引用路径
- 判断什么时候应该更新 spec、ADR 或 progress

## 它会教 Agent 什么

这个 skill 会建立一套通用文档架构：

```text
docs/
  README.md                 # 文档地图和读取顺序
  progress.md               # 当前事实、活跃工作、红线提醒
  decisions_index.md        # ADR 状态索引
  decisions.md              # 完整 ADR 历史
  specs/                    # 当前模块契约
  runbooks/                 # 操作和验证流程
  research/                 # 研究资料入口
  archive/                  # 已废弃计划和历史日志
```

同时定义这些规则：

- 什么时候更新 specs
- 什么时候写或更新 ADR
- 如何让 progress 文档保持短小
- 如何处理高风险文件的 guardrail
- 如何安全迁移文档并避免断链

## 安装

这个仓库不绑定具体工具。任何支持可复用 instructions、skills、rules、memory 或 knowledge files 的 Agent 都可以用。

### 方式 A：作为 skill 文件夹使用

把 skill 文件夹复制到你的 Agent skills / instructions 目录：

```bash
cp -R document-governance /path/to/your/agent/skills/
```

然后开启新的 Agent 会话，让它使用 `document-governance`。

### 方式 B：复制到长期指令

打开 `document-governance/SKILL.md`，把内容粘贴到你的 Agent 长期指令、自定义规则或知识库中。

### 方式 C：直接引用

让 Agent 读取 `document-governance/SKILL.md`，并按里面的流程执行。

## 示例 Prompt

```text
Use document-governance to reorganize this repo's docs folder.
```

```text
Audit our docs and propose a low-token reading order for future agents.
```

```text
Create a docs README, ADR index, specs README, runbooks README, and archive README.
```

```text
Decide which docs should be current-state docs, specs, ADRs, runbooks, research, or archive.
```

中文也可以这样说：

```text
使用 document-governance 帮我整理这个项目的文档结构，降低以后 AI 读取文档的 token 成本。
```

```text
审计这个仓库的 docs 目录，告诉我哪些文档应该保留为入口，哪些应该归档。
```

## 仓库结构

```text
document-governance/
  SKILL.md
```

## 说明

这个 skill 是项目中立的，不绑定任何具体技术栈、公司、产品或仓库结构。

## License

MIT
