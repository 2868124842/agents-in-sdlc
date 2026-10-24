---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可在云端异步处理任务。您在 GitHub 上分配工作后，它会在后台处理任务：探索仓库、进行更改并创建拉取请求，而您可以继续处理其他事情。

在这些练习中，您将添加云智能体遵循的自定义说明，然后分配 GitHub issue 并让它实现任务。您将评审并使用自定义代理来指导其工作方式，通过代理仪表板监控和引导会话，最后评审其拉取请求并迭代完善结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建仓库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云代理将遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步代理 | 将 issue 分配给 Copilot 云智能体 |
| [3. 自定义代理][ex3] | 专用代理 | 评审并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代完善][ex5] | 评审 | 评审 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确认您具备：

- [ ] 有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为您的帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的代理、MCP、代码评审和 Copilot CLI 功能，因此可完成所有练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云智能体；只有 Copilot Free 不支持。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用此功能。

> [!NOTE]
> 本练习不涉及 MCP。如需了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 练习][cli-harness]或 [VS Code 练习][vscode-harness]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
