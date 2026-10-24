---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起评审筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支中。现在应将它们一起评审并创建拉取请求。您已经亲自合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 代理拆分到几个 PR 中。为简化本工作坊，筛选功能和质量工作流都在同一对话和分支中完成，并包含在此 PR 中。

本课将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在评审完成后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个工作流程中，您使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余的 PR 工作，同时由开发人员控制合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复评审评论，以及在需要时进行变基；之后，如果仓库允许，它会启用 GitHub 自动合并。

此前，您都是自己选择 **Merge pull request**。现在可以将这项工作交给 Agent Merge，但启用前仍应先评审更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码全部完成后，一起评审、创建 PR，再让 Agent Merge 管理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必要的审批、分支保护、合并队列、仓库设置或权限限制。如果操作受阻，请查看报告的原因；在仓库允许的情况下，完成评审后手动合并。

## 总结和后续步骤

您已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，您：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整功能更改和验证证据。
- 创建了筛选功能 PR。
- 评审后启用了 Agent Merge，并确认 PR 已合并。

接下来，探索更多实用的 Copilot CLI 斜杠命令，以管理上下文、模型和共享，并选择是否委派任务到云端。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
