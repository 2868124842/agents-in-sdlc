---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要更改多个文件并执行运行测试等操作。开发人员通常需要找到所有相关文件、修改代码、运行测试、调试、找出遗漏的文件、再次更新……后续步骤还有很多。

这正是 Copilot 代理模式的用武之地。

Copilot 代理模式可在 IDE 中以更高的自主性工作。它的行为类似开发人员：先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 名称相似，但代理模式和云智能体面向两种不同的使用体验。代理模式在 IDE 中执行任务，可快速获得反馈并进行交互。云智能体则像团队成员一样异步工作，通过 issues 和拉取请求与您协作。

本练习将学习：

- Copilot 代理模式如何探索项目、找到相关文件并协调更改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 如何在将 Copilot 代理模式生成的更改合并到代码库前评审更改和测试。

## 场景

随着游戏列表增长，您希望允许用户按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先浏览 Tailspin Toys 网站，了解其当前功能。

网站是一个以开发者为主题的桌游众筹平台。用户可以查看游戏列表及其详情。该网站是单个 Astro 应用，构建时会将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可重用的数据访问辅助函数位于 `src/lib/`，交互功能则使用简短、作用域明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动单个 Astro 开发服务器。在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。上一练习后，您应已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，现在您的 Web 服务器正在该环境中运行。

### 浏览网站

网站运行后，可以探索其功能。网站的主要功能包括：

- **首页**：显示桌游列表，包括标题、图像和简介。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、简介、发行商和类别等信息。

## 使用 Copilot 浏览待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。每个练习对模式和模型的要求可能不同；相关步骤会在适用时说明。

网站的初始实现已经可以运行，但我们希望添加新功能。先浏览待办列表。使用模板创建项目时，系统已自动创建一组 issue；现在让 GitHub Copilot 显示这些事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令以及调用工具。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好任何特定模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此在说明中列出了该模型，希望您获得的代码建议能相对一致。由于大型语言模型（LLM）的输出具有概率性，实际建议可能与工作坊示例不同。这完全正常。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办 issue：

   ```plaintext
   请显示我的 GitHub 仓库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```

4. 选择 **Continue** 运行列出所有 issue 的命令。
5. 评审生成的 issue 列表。

留意 Copilot 还会根据它认为对用户最有用的程度为事项排序。

## 评审说明文件

让代理生成代码前，先查看将用于提供工作上下文的说明文件。此处将使用[用户界面（UI）说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中记录了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意添加功能时的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以重用说明、将复杂内容拆分为更容易管理的小文件，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三处独立更新：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 代理模式可以完成这些任务。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办列表中相关 issue 的要求实现功能：

   ```plaintext
   请根据待办列表中相关 GitHub issue 的要求，更新网站，添加按发行商和类别筛选的功能。上一练习中已创建发行商辅助函数；请保留并根据需要完善它，同时完成剩余的数据层、UI 和测试工作。完成前确保所有测试均通过。服务器已在运行，无需重新启动。
   ```

6. 观察 Copilot 如何先探索项目，找到相关功能文件，包括数据层辅助函数、UI 和测试。随后它会修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会有 **Keep** 或 **Undo** 选项，因此无需在工作过程中立即选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中要求用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停几次，某些测试也可能暂时失败。这没关系。Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再发现错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找可能的问题。

> [!CAUTION]
> 请务必评审 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，探索新功能。
11. 确认一切正常并完成代码评审后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，您了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。您学会了：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 在将 Copilot 代理模式生成的更改合并到代码库前，评审更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表增长，需要添加分页。运用本练习学到的技能，让 Copilot 更新网站并实现分页。编写提示和代码时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 考虑分页方式，例如允许用户选择每页条数，或使用固定值。
- 编写提示时提供必要指导，确保 Copilot 按预期实现功能。
- 可能需要与 GitHub Copilot 多次迭代，请求更改并提供上下文。这是常规协作方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
