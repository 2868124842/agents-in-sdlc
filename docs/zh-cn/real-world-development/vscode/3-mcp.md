---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

您刚刚使用代理模式构建了筛选功能。在创建拉取请求之前，应先确认功能在浏览器中确实正常运行。接下来将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，而不是由您亲自逐项检查。之后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 在工作区中添加 Playwright MCP 服务器。
- 请求 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

代理模式如果能够使用编辑器之外的工具，功能会强大得多。Copilot 通过 Model Context Protocol (MCP) 实现这一点：这是一种让代理与外部工具和服务通信的标准方式。

![示意图：代理模式的内部工作方式，以及它如何与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供与外部工具和服务通信的方式。AI 代理可以使用 MCP 实时访问最新信息（通过资源），并代表您执行操作（通过工具）。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器负责管理 AI 代理与外部工具之间的通信，例如现有 API 或 NPM 本地工具包。每个 MCP 服务器都代表一组可供 AI 代理使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 仓库的 API，允许 AI 代理创建仓库、更新现有仓库，以及管理 issues 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，允许 AI 代理浏览网页、填写表单和选择按钮。

此外还有许多其他 MCP 服务器，可提供不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，以提高生态系统的可发现性并促进贡献。

> [!CAUTION]
> 出于安全考虑，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，仔细查看其源代码、验证发布者并考虑相关安全影响。只使用可信赖的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置了此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 您应该会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含 `github` 配置，因此上一练习中 Copilot 才能读取待办 issue。此配置使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接该服务器，Copilot 首次使用其工具时需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控制的浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在您的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记要求 Playwright 在没有可见窗口的情况下运行浏览器。这对于没有桌面环境可显示窗口的 codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需启动** MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并要求您确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 要求确认是否信任服务器，请先检查配置，然后选择信任，以便启动服务器。

手动启动是可选的。如果跳过，下一步中 Copilot 首次需要服务器工具时，VS Code 会自动启动服务器并提示您确认是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可以驱动的真实浏览器。您无需亲自在应用中逐项检查，代理可以打开页面、导航、应用筛选器、读取结果并向您总结。无需离开当前对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]而非截图进行操作。这意味着代理会像辅助技术一样，依据带标签的结构化元素（按钮、链接和列表项）进行推理。因此，简单的功能检查也能顺带进行基本的无障碍检查。

服务器已连接且应用正在运行后，请求 Copilot 测试刚构建的筛选功能：

```text
使用 Playwright MCP 服务器打开浏览器，访问正在运行的应用 http://localhost:4321，并验证新游戏筛选功能：

1. 打开游戏页面，记录列出的游戏数量。
2. 应用类别筛选器，确认列表只显示该类别中的游戏。
3. 清除筛选器，然后应用发行商筛选器，确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选器，确认结果符合两项筛选条件。

报告每一步观察到的情况，并指出任何不符合预期的行为。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行检查并报告结果。请对照 issue 中的验收标准阅读摘要。如果发现问题，可继续提问或让 Copilot 修复代码，再创建拉取请求。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请先重新启动再发送提示。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队评审。第一步是发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并评审 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。既然已经在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关 issue 并创建与该 issue 关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认代理下拉菜单中选中 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 请求 Copilot 创建 PR：

   ```plaintext
   找到仓库中与按类别和发行商筛选相关的 issue。为当前分支创建新的拉取请求，并将其关联到正确的 issue。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会提示您通过 GitHub 身份验证；请按提示完成操作。
5. 留意 Copilot 如何搜索 issues、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但暂时**不要合并**。

## 总结和后续步骤

恭喜！本练习中，您：

- 了解了 Model Context Protocol (MCP) 及 MCP 服务器如何扩展 Copilot 工具。
- 在工作区中添加了 Playwright MCP 服务器。
- 在发布功能前使用它通过浏览器手动测试了筛选功能。
- 发布了分支并为筛选功能创建了拉取请求。

您使用 MCP 服务器**测试**了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，以简化无障碍评审等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
