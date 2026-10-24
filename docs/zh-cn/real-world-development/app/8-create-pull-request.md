---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并评审筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支中。现在应将它们一起评审并创建拉取请求。您已经亲自合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 代理拆分到几个 PR 中。为简化本工作坊，筛选功能和质量工作流都在同一会话和分支中完成，并包含在此 PR 中。

本课将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整功能 PR 和验证证据。
- 仅在评审完成后授权 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个工作流程中，您使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时仍由开发人员控制合并授权。

## 了解 Agent Merge

**Agent Merge** 可在 GitHub Copilot app 中自动完成合并拉取请求所需的剩余工作。启用后，应用会话会读取 PR 并处理阻塞项，例如修复失败的持续集成（CI）检查、回复评审评论，以及在需要时进行变基。GitHub 允许后，它会立即合并。此功能在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

此前，您都是自己选择 **Merge pull request**。现在可以将这项工作交给 Agent Merge，但仍需明确授权它编辑代码和合并。授予合并权限前，请先评审其允许执行的操作和工作内容。

## 使用 Agent Merge 管理 PR

代码全部完成并评审后，允许 Agent Merge 管理 PR 流程。

1. 使用代理选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮开始流程。仅从下拉菜单中选择该选项并不会启动流程。

Agent Merge 流程将开始执行以下操作：

- 使用标题和说明创建拉取请求。
- 如果会话从 issue 启动，则在说明正文中引用相关 issue。
- 对目标分支进行变基，或处理潜在的合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控其他开发人员的评审反馈或 Copilot 代码评审结果，并进行更新以解决相关评论。
- 可选择在所有检查成功后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，评审 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求代理执行合并：

    ```plaintext
    所有必需的检查和审批均通过后再合并 PR。不要绕过仓库保护措施。
    ```

9. 跟进执行进度并解决报告的阻塞项。继续操作前，确认 PR 状态为 **Merged**；仅启用合并权限或代理仍在运行，都不能证明合并已经完成。

> [!IMPORTANT]
> Agent Merge 不会绕过仓库保护措施或权限限制。继续前请先解决这些阻塞项。

## 总结和后续步骤

您已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，您：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整功能 PR 和验证证据。
- 评审后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分类画布][next-lesson]，探索如何与代理共同检查、规划和可视化工作。

## 资源

- [使用 GitHub Copilot app 管理 issues 和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
