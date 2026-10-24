---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将 issue 要求、quality-checks 技能和 Playwright MCP 整合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

您已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个自定义代理中，并为它定义清晰的 QA 流程。

本课将：

- 了解自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查可重用的质量保证配置。
- 选择 QA 代理，并根据筛选 issue 评审它的发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，对需求、代码质量、自动化检查、测试覆盖率和浏览器行为进行一致的评审。自定义代理可以协调 QA 流程，并生成可重复使用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件描述代理的用途、说明和可用工具。在本工作坊中，您将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

您使用的自定义功能各有不同用途：仓库说明描述团队标准；`quality-checks` 技能打包可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 使用这些能力来评估需求并报告发现。它不会取代其他自定义功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，先让 Copilot 创建可重用的 QA 配置文件。文件将定义 QA 执行的检查以及必须遵循的限制。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请求默认代理创建自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建一个名为 QA 的自定义代理。它应根据 issue 和已商定的要求检查功能、遵循仓库说明、运行 quality-checks 技能、使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它为每项要求报告通过、失败或受阻，并提供支持证据。修改实现代码前必须先询问；不得提交更改或创建拉取请求。

   现在只创建配置文件，供我评审。
   ```

## 检查配置文件

使用新代理前，先评审其配置，确认 Copilot 已记录预期的 QA 工作流程和限制。这样可以避免代理在您只想验证功能时进行不完整或范围过广的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 为可选字段，但添加它可以作为清晰的显示名称。
3. 阅读代理说明，确认 QA 会从需求开始、遵循仓库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述职责或限制，请要求默认代理修订后再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一筛选对话，再选择 QA，这样它就能使用已有的 issue 和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 您刚创建的代理可能不会立即显示在可用代理列表中，但仍可使用上述命令启用。

2. 使用以下提示，请 QA 代理评审功能：

   ```plaintext
   根据 issue 和计划中确定的决定评审筛选功能。它可以准备好创建 PR 了吗？
   ```

3. QA 代理开始工作。
4. 工作完成后，阅读它提供的报告。

## 总结和后续步骤

您已向工作流程添加了可重用的专用角色，并评审了它的工作。本课中，您：

- 了解自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查可重用的质量保证配置。
- 选择 QA 代理，并根据筛选 issue 评审它的发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好供评审。接下来，将它们整合到功能 PR 中并使用 Agent Merge。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
