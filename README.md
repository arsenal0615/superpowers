# Superpowers

Superpowers 是一套完整的软件开发工作流系统，专为 AI 编程助手设计。它基于一组可组合的"技能（skills）"和一套初始指令，确保你的编程助手能够正确使用这些技能。

## 工作原理

从你启动编程助手的那一刻起，它就开始发挥作用。当它发现你正在构建某个功能时，它**不会**直接跳到写代码的步骤，而是先退一步，问清楚你到底想做什么。

当它从对话中梳理出需求规格后，会把设计方案分成足够短小的段落展示给你，确保你能真正阅读和理解。

在你确认设计方案后，编程助手会制定一份实施计划——清晰到连一个热情但缺乏品味、缺乏判断力、没有项目背景、还不爱写测试的初级工程师都能照着做。它强调真正的红/绿 TDD、YAGNI（你不会需要它）和 DRY（不要重复自己）。

接下来，当你说"开始"后，它会启动 *subagent-driven-development*（子代理驱动开发）流程，让多个代理逐一完成每个工程任务，检查和审查它们的工作，然后继续推进。Claude 连续自主工作几个小时而不偏离你制定的计划，这是很常见的。

系统还有更多功能，但以上就是核心流程。由于技能是自动触发的，你不需要做任何特别的事情。你的编程助手就自带了 Superpowers。


## 赞助

如果 Superpowers 帮助你做了赚钱的事情，并且你有这个意愿，非常感谢你考虑[赞助我的开源工作](https://github.com/sponsors/obra)。

谢谢！

- Jesse


## 安装

**注意：** 不同平台的安装方式不同。Claude Code 和 Cursor 有内置的插件市场。Codex 和 OpenCode 需要手动设置。


### Claude Code（通过插件市场）

在 Claude Code 中，先注册插件市场：

```bash
/plugin marketplace add arsenal0615/superpowers-marketplace
```

然后从该市场安装插件：

```bash
/plugin install superpowers@qx-superpowers-marketplace
```

### Cursor（通过插件市场）

在 Cursor Agent 对话中安装：

```text
/plugin-add superpowers
```

### Codex

告诉 Codex：

```
Fetch and follow instructions from https://raw.githubusercontent.com/arsenal0615/superpowers/refs/heads/main/.codex/INSTALL.md
```

**详细文档：** [docs/README.codex.md](docs/README.codex.md)

### OpenCode

告诉 OpenCode：

```
Fetch and follow instructions from https://raw.githubusercontent.com/arsenal0615/superpowers/refs/heads/main/.opencode/INSTALL.md
```

**详细文档：** [docs/README.opencode.md](docs/README.opencode.md)

### 验证安装

启动一个新会话，询问一些应该触发技能的内容（例如"帮我规划这个功能"或"让我们调试这个问题"）。编程助手应该会自动调用相关的 superpowers 技能。

## 基本工作流

1. **brainstorming（头脑风暴）** - 在写代码之前激活。通过提问来完善粗略的想法，探索替代方案，分段展示设计方案供验证。保存设计文档。

2. **using-git-worktrees（使用 Git 工作树）** - 在设计方案获批后激活。在新分支上创建隔离工作空间，运行项目设置，验证测试基线正常。

3. **writing-plans（编写计划）** - 在设计方案获批后激活。将工作拆分为小任务（每个 2-5 分钟）。每个任务都有精确的文件路径、完整的代码和验证步骤。

4. **subagent-driven-development（子代理驱动开发）** 或 **executing-plans（执行计划）** - 有计划后激活。为每个任务分派新的子代理，进行两阶段审查（先检查规格合规性，再检查代码质量），或者分批执行并在人工检查点暂停。

5. **test-driven-development（测试驱动开发）** - 在实现过程中激活。强制执行 RED-GREEN-REFACTOR：先写失败的测试，看它失败，写最少的代码，看它通过，提交。删除在测试之前写的代码。

6. **requesting-code-review（请求代码审查）** - 在任务之间激活。对照计划进行审查，按严重程度报告问题。关键问题会阻止继续推进。

7. **finishing-a-development-branch（完成开发分支）** - 在所有任务完成后激活。验证测试，提供选项（合并/PR/保留/丢弃），清理工作树。

**编程助手在执行任何任务之前都会检查相关技能。** 这是强制性的工作流，不是建议。

### **⭐ Change Mode（变更模式 - 结构化生命周期）**

> **这是 Superpowers 最核心的工作流，类似于 OpenSpec 的规格驱动开发模式。** 它将软件开发的完整生命周期——从需求提出到最终归档——组织成一条清晰的流水线，确保每一步都有据可循、可追溯。

**适用场景：** 较大的功能开发、跨角色交接（设计师 → 工程师）、需要需求跟踪的工作，或任何你希望有**完整文档记录**的变更。

**完整生命周期：`提议 → 规格 → 设计 → 计划 → 执行 → 验证 → 归档`**

1. **exploring（探索）** - 自由形式的思考伙伴。没有强制输出，没有固定步骤。调查代码库，比较选项，用 ASCII 图表进行可视化。随时退出或将想法**结晶为一个变更**。

2. **managing-changes（管理变更）** - **核心引擎。** 在 `docs/changes/<name>/` 创建变更目录，自动生成完整的工件链：
   - **`.change.yaml`** - 变更元数据和状态跟踪
   - **`proposal.md`** - 变更提议（为什么要做）
   - **`specs/`** - 需求规格（做什么）
   - **`design.md`** - 技术设计（怎么做）
   - **`plan.md`** - 实施计划（带持久化复选框，支持跨会话恢复进度）

3. **writing-plans / executing-plans** - 与快速模式相同的执行技能，但所有输出到变更目录中。**计划中的复选框会持久化保存**，即使会话中断也能从上次停下的地方继续。

4. **验证与归档** - 变更完成后进行验证，通过后归档到 `docs/changes/archive/`。**规格会自动积累到 `docs/specs/` 中**，形成项目的活跃需求库。

**目录约定：**
- **`docs/changes/`** - 包含提议、设计、规格和计划的活跃变更
- **`docs/changes/archive/`** - 以日期前缀归档的已完成变更
- **`docs/specs/`** - 项目级需求规格，在变更归档时自动更新

## 包含内容

### 技能库

**测试**
- **test-driven-development** - RED-GREEN-REFACTOR 循环（包含测试反模式参考）

**调试**
- **systematic-debugging** - 4 阶段根因分析流程（包含根因追踪、纵深防御、条件等待技术）
- **verification-before-completion** - 确保问题真正被修复

**协作**
- **exploring** - 自由形式的思考伙伴，用于想法、问题和技术调查
- **managing-changes** - 变更生命周期管理（提议、规格、设计、验证、归档）
- **brainstorming** - 苏格拉底式设计完善（快速模式）
- **writing-plans** - 详细的实施计划（支持变更模式的持久化复选框）
- **executing-plans** - 带检查点的批量执行（支持跨会话进度恢复）
- **dispatching-parallel-agents** - 并发子代理工作流
- **requesting-code-review** - 提交前审查清单
- **receiving-code-review** - 响应反馈
- **using-git-worktrees** - 并行开发分支
- **finishing-a-development-branch** - 合并/PR 决策工作流
- **subagent-driven-development** - 快速迭代，两阶段审查（规格合规性 + 代码质量）

**元技能**
- **writing-skills** - 按照最佳实践创建新技能（包含测试方法论）
- **using-superpowers** - 技能系统介绍

## 理念

- **测试驱动开发** - 永远先写测试
- **系统化优于即兴** - 流程优于猜测
- **降低复杂度** - 简洁是首要目标
- **证据优于声明** - 在宣布成功前先验证

了解更多：[Superpowers for Claude Code](https://blog.fsck.com/2025/10/09/superpowers/)

## 贡献

技能直接存放在本仓库中。贡献方式：

1. Fork 本仓库
2. 为你的技能创建一个分支
3. 按照 `writing-skills` 技能来创建和测试新技能
4. 提交 PR

完整指南请参见 `skills/writing-skills/SKILL.md`。

## 更新

更新插件时技能会自动更新：

```bash
/plugin update superpowers
```

## 许可证

MIT 许可证 - 详见 LICENSE 文件

## 支持

- **Issues**: https://github.com/arsenal0615/superpowers/issues
- **Marketplace**: https://github.com/arsenal0615/superpowers-marketplace
