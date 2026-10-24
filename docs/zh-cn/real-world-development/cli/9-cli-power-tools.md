---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令提供高级功能、后台信息和额外配置选项。您已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来看看其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派到云智能体的可选命令。

## 场景

核心 CLI 工作流程已经完成。现在来看看其他能力：管理上下文、切换模型、共享会话，以及选择是否将任务委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型的最大上下文窗口。需要时，Copilot CLI 会自动压缩对话；您也可以使用斜杠命令检查或手动压缩上下文。

1. 返回 Codespace。如果尚未打开 Copilot CLI，请从仓库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 用量，以及上下文在系统说明、工具、消息和剩余空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口逐渐填满时自动压缩上下文。如果您想自行选择压缩时机，可以使用 `/compact`。如果要切换到无关任务并开始全新的对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有各自的偏好。Copilot CLI 可以列出可用模型并让您选择。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保持当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看所安装版本中的 `/share` 选项。
2. 如需共享会话，请选择适合的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 将导出内容发送给他人或发布前，请先检查内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。请勿发布团队不打算共享的仓库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以将相互独立的子任务分配给子代理并行执行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体，由它异步处理并可能创建拉取请求。

这些命令都是可选的，因为它们可能创建额外工作树或触发远程工作。尝试前，请从范围明确的新任务开始，并按照常规工作流程评审结果。若要深入了解异步代理工作，请继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并查看后台状态。本课中，您：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委派到云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。最后，让我们[回顾所学内容][next-lesson]并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
