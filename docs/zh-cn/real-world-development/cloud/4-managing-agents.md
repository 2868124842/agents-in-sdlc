---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，您让 Copilot 云智能体处理了三项任务，以改善用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

有多种工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，您可以查看所有具有未关闭拉取请求（PR）的代理任务，了解已执行的操作，甚至可以引导正在运行的会话。

本课将：

- 浏览代理页面，监控云智能体任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

将任务分配给代理实现高对比度模式后，团队发现也可以借此添加浅色模式。既然已经在更新网站样式并添加切换功能，不妨将其一并实现。您将引导代理，要求它在高对比度模式之外再添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。您应该能看到已分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从仓库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 完成分配任务的各种步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 同时留意右侧显示的拉取请求（PR）窗格。您可以通过此窗格查看 PR 和已更改文件，以便进一步监控。

## 引导云智能体

现在您已了解正在运行的任务，接下来让 Copilot 在实现高对比度模式的同时添加浅色模式切换。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 继续查看会话几分钟，直到显示设置已完成并开始执行任务。此时会出现与以下内容类似的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中突出显示“Steer active session while Copilot is working”对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成发送消息时正在执行的步骤，然后将新要求纳入工作流程。

这同样需要几分钟。等待期间，可以暂停一下，回顾到目前为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过这个任务控制中心，您可以跟踪所有正在运行和已完成的任务，评审正在执行的工作，还可以重定向进行中的任务以调整范围或提供进一步指导。

您了解了以下概念：

- 使用任务控制中心和代理页面监控云智能体任务。
- 重定向正在运行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，我们将[迭代完善 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
