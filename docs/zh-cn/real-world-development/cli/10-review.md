---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可重用的自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

您已在 Tailspin Toys 的完整工作流程中使用 GitHub Copilot CLI。您完成了以下工作：

- 准备 Codespace、安装 Copilot CLI、浏览项目并找到已填充的筛选 issue。
- 添加星级评分，在转发的浏览器中评审结果，并手动合并第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下评审。
- 使用自定义说明引导代理，然后自定义现有 `quality-checks` 技能，运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并使用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、测试覆盖率、技能结果和浏览器验证证据。
- 评审完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选委派到云端的斜杠命令。

## 您交付的内容

本工作坊包含两个 PR 阶段，每个阶段都从更新后的 `main` 创建独立分支：

1. **星级评分：** 在游戏卡片上显示已有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：** 实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建其 PR，您一直使用相同的对话和分支。为简化工作坊，我们将这些更改合并在一个 PR 中。

## 不同类型的验证

您通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 浏览器探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 则在 PR 创建前，将这些结果与需求和测试覆盖率评审结合起来。

新增测试应当填补实际缺口；如果 QA 运行后判断无需添加测试，也可能是合理结果。授权合并前，请评审代码和证据。代码更改后，应重新检查受影响的证据。

## 最佳实践

您提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，您更新了说明、自定义了技能、创建了 QA 配置文件并设置了 MCP 服务器。可以在不同会话中重用这些自定义内容，并根据团队需求进行调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器则连接外部工具。请检查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前思考方案；使用 **Interactive** 逐步处理聚焦的更改；对范围明确的任务使用 **Autopilot**。例行编辑可选择速度更快的模型，复杂工作则选择能力更强的模型。

上下文和基础设施同样重要。清楚说明要构建什么、为什么要构建，以及应如何构建，会显著影响输出结果。

## 更多探索

核心工作流程已经涵盖。还可以了解几个 Copilot CLI 功能：

- 使用 `/review` 请求代码评审代理分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种思路。
- 使用 `/fleet` 并行编排相互独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方式就是持续实践！可以将 Copilot 用于生产代码、兴趣项目，或多年来一直想构建但还没动手的小应用。与团队分享经验，也向团队成员学习。还要记得持续探索文档。

如果想从终端继续扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。如需比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
