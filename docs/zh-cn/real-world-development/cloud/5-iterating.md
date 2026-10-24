---
title: "练习 5：迭代完善 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 评审工作

在本实验中，您与 GitHub Copilot 一起处理了多项任务，以改善用户体验并添加功能。您让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，并实现包括高对比度和浅色模式切换在内的无障碍功能。现在，我们来查看代码更改；如有必要，也可以向 Copilot 提供反馈以改进成果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要评审生成的代码，并遵循常规 DevOps 流程。因此，在为团队其他成员启用评审之前，我们先评审 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能生成的方案。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会在无人监督的情况下异步处理任务，因此设置了一些安全限制，确保工作过程安全：

- Copilot 对仓库只有读取权限，只能写入用于存放其代码的分支。
- 云智能体在 GitHub Actions 中运行，并会创建独立的临时环境。
- 任何 GitHub Actions 工作流都需要经过人工批准才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 评审生成的文档

先查看 GitHub Copilot 云智能体生成的第一个拉取请求（PR），其中包含为代码添加的文档。此处将使用 GitHub.com 的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，您可能会看到提示，说明 GitHub Copilot 被防火墙阻止。这是**正常现象**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如果愿意，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完善的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成任务，这可能需要几分钟。您可以稍作休息，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并评审更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些内容来自您在练习 2 开始时查看的自定义说明文件。

6. 评审更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查区域排队运行。正常情况下，单个 Astro 应用的项目检查会通过。这可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作并非单向过程。您可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于输出具有非确定性，我们无法提供唯一、固定的提示文本。您可以要求 Copilot：

- 在每个代码文件顶部添加注释标题，简要说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 在拉取请求中添加评论并提及 **@copilot**，请求它更改生成的文档。可以采用上述建议，也可以提出您希望代码库包含的其他文档要求。
2. 选择 **View Session**，查看 Copilot 如何处理请求。留意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡评审更改。

可以继续迭代，直到满意为止。满意后，可将 PR 从草稿状态转换为就绪状态，再合并到主分支。

![将 PR 转换为就绪状态](../../../_images/ex7-ready-for-review.png)

## 评审相关游戏功能

现在回到 Copilot 为解决“在游戏详情页显示相关游戏”这一 issue 而创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完善的 PR。
4. 选择 **Files changed** 选项卡，评审生成的代码。
5. 评审更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查区域排队运行。正常情况下，单个 Astro 应用的项目检查会通过。这可能需要几分钟。
9. **可选：** 您可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace，启动终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，您可以继续迭代并请求云智能体更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释标题和 TSDoc 文档注释（请记住，分配该任务时，您还没有更新自定义说明）。同样，可以在 **Conversation** 选项卡中添加评论提出请求，Copilot 会读取评论并启动新会话。

## 评审无障碍功能

最后，评审使用无障碍自定义代理实现的功能。此 PR 应同时包含您在练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中请求的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，评审生成的代码。
5. 评审实现时，重点关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 评审更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查区域排队运行。正常情况下，单个 Astro 应用的项目检查会通过。这可能需要几分钟。
10. **可选：** 您可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace，启动终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认其功能正常，并且页面重新加载后设置仍然保留。

留意无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进之处，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理真实待办事项，让您可以转而完成其他工作。可以在自己的仓库中创建更多 issue 并分配给 Copilot，练习这种协作方式。可尝试以下任务：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

您已完成云智能体练习。在这些课程中，您：

- **查看了仓库提供的自定义说明文件**，了解它们如何影响云智能体的输出。
- **将 issue 分配给 Copilot 云智能体**，观察它如何设置环境、规划并异步执行任务。
- **创建并使用了无障碍自定义代理**，添加高对比度和浅色模式切换。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导正在运行的无障碍会话。
- **评审并迭代完善了云智能体创建的拉取请求**，提及 `@copilot` 请求更改并批准工作流。

## 复习和后续步骤

您已完成云智能体练习。如果想继续探索，其他练习可以补充您已掌握的内容：

- 🖥️ **[VS Code 练习](../../vscode/)** — 在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)** — 从终端交付两项经过评审的更改，体验 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体交接。

也可以在自己的仓库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云智能体。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issues 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
