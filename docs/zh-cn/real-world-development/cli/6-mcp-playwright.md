---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们需要处理数据和外部服务，有时还要让 Copilot 使用其他自动化工具。MCP 服务器可以帮助 Copilot 超越 CLI 内置功能，使用更多工具和服务。

本课将：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚不可用时添加它。
- 请求代理驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证 UI 更新还需要实际与 UI 交互。您希望让 Copilot 像用户一样使用正在开发的网站，以便进一步自动化更改流程，并更有把握地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post]为 AI 代理提供与外部工具和服务通信的方式。AI 代理可使用 MCP 实时访问外部工具和服务，从而获取最新信息并代表您执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁。每个 MCP 服务器都代表一组可供 AI 代理使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、issues 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，以提高生态系统的可发现性并促进贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，先查看其源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前您已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器执行相同的检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照 issue 和我们的计划检查筛选功能。不要进行任何更改，告诉我哪些功能正常、哪些不正常。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，因此不一定要明确指示它。不过，既然您知道它应该使用什么工具，明确指出也无妨。这能让结果更一致，也可以节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的步骤，以确认功能是否正常。
3. 阅读报告，确认各项行为符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器并向您报告结果。

## 总结和后续步骤

恭喜，您已在 Copilot CLI 中使用 Playwright MCP 服务器通过真实浏览器探索功能。回顾一下，您：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚不可用时添加了它。
- 请求代理驱动浏览器并探索筛选功能。

接下来，[创建 QA 自定义代理][next-lesson]，将技能和浏览器工具结合起来，形成专门的 QA 角色。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
