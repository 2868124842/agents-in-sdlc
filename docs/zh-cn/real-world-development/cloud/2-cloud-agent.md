---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺乏时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发人员评审。这样可以把繁琐的任务交给代理，加快开发进程，让开发人员专注于更重要的工作。

本练习将使用 Copilot 云智能体来：

- 自定义代码生成环境。
- 确保操作安全。
- 了解 issue 范围清晰的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些希望解决的技术债务。最初负责创建网站的承包商没有妥善处理文档——事实上，您会发现文档完全缺失。首先，团队希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望游戏详情页显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。当前不需要精致的实现，只要能用于 UX 验收测试即可。这项工作目前是一个阻碍，但还有其他优先级更高的 issue。

这两项工作都可能很快被搁置，因此适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员先做其他工作，再回来评审 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人工开发者一样在后台处理任务。您可以通过多种方式分配工作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。您可以在会话运行期间或完成后查看 Copilot 的工作。准备好评审方案时，它会在拉取请求中通知您。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法，不能代替思考。即使看似简单的操作，深入了解后也可能变得复杂，因此要[谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 编程伙伴：分阶段工作、学习、试验并不断调整。使用生成式 AI 并不会改变软件开发的基本原则。

## 此仓库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的指导作用。如果您已经完成自定义说明的练习，就见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速浏览一下。分配工作前，请只读查看仓库中已有的说明文件，之后就能留意到它们的影响。

在仓库的 GitHub 网页界面中，或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

下一节中，将 `Code lacks documentation` issue 分配给云智能体后，留意生成的拉取请求中是否有 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些内容来自上述说明文件。稍后的练习会再次介绍这一点。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境处于正常状态。将任务分配给 Copilot 时也是如此；它会以类似软件工程师的方式执行任务。

云智能体使用 [GitHub Actions][github-actions] 运行工作。您可以创建[专用设置工作流][setup-workflow]来定制其环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在云智能体开始工作前运行。实验已预先配置此工作流，以便顺利完成流程并体验相关功能。它会确保 Copilot 可使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的单个 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从仓库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 只授予步骤所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果要在设置步骤中克隆仓库（例如安装依赖项），需要 contents: read 权限。如果不克隆仓库，Copilot 会在步骤完成后自动为您执行此操作。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，使构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来与其他 GitHub 工作流相同，但有几个要点：

- 它只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许您从仓库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功，而无需等待 Copilot 触发它。

## 添加文档

尽管大家都了解文档的重要性，许多项目的文档不是过时就是缺失。这类技术债务常常得不到处理，拖慢效率，使代码库更难维护，也让新开发人员更难加入团队。Copilot 擅长创建文档，因此这是适合分配给云智能体的任务。它会在后台生成所需文档，您将在后续练习中回来评审成果。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新的 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 评审列表及 Copilot 准备完成的任务。
14. 向下滚动查看拉取请求时间线，留意 Copilot 已开始处理 issue 的提示。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 您可能需要刷新窗口，才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步处理任务，让我们能专注于其他工作。接下来，我们会利用这一点，再分配一项任务给云智能体，然后转而编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏版块

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：您专注于一项任务，代理则处理另一项任务。为设计团队添加相关游戏版块也许不会花太长时间，但这段时间仍可用于其他工作。现在就把任务分配给云智能体吧。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择空白模板 **Blank issue**。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，您会在 issue 的第一条评论中看到 👀，表示 Copilot 已开始处理。

![Copilot 用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此您不必持续监控，只需在任务完成后进行评审即可。接下来，我们来创建并使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。您可以将 issue 分配给云智能体，让它异步处理技术债务、开发新功能，或协助将代码从一个框架迁移到另一个框架。

您了解了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云智能体正在后台工作，现在可以转而创建并使用自定义代理。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并支持自定义说明；您已在之前的模块中了解过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issues 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
