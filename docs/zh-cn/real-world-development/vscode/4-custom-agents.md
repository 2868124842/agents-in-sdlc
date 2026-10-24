---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]可让您创建专门面向开发工作流中特定任务或领域的 AI 助手。在仓库的 `.github/agents` 文件夹中通过 Markdown 文件定义代理，即可为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，引导它更有效地完成特定类型的工作。团队可以将专业知识整理为可重复使用的代理，例如确保符合 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理在项目的 `.github/agents` 文件夹中通过 Markdown 文件定义，也可以在全局位置 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description`，后面是定义代理行为、专业领域和说明的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理和[代理技能][agent-skills-concept]在概念上有一些重叠。它们主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的角色，技能是可用的工具**。

自定义代理有自己的上下文窗口，能够在工作过程中编排技能（甚至其他代理）。在本实验中，无障碍自定义代理会依据无障碍指南评审并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能来协助完成任务。

> [!NOTE]
> 自定义代理没有唯一正确的写法。与 AI 中的其他做法一样，请在自己的环境和场景中测试并迭代调整。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，一些用户觉得当前暗色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现一种可由用户切换的高对比度模式。

无障碍访问至关重要，因此需要尽快实现此功能。您将使用自定义代理生成相关功能。

本练习将：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 评审无障碍自定义代理

已为您创建无障碍自定义代理。先查看其内容，了解它将如何指导 Copilot。

返回 codespace，打开终端并从 `main` 创建新分支，专门处理无障碍功能（将练习 3 的筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含带 `name` 和 `description` 的 frontmatter。

3. 继续浏览后续部分，了解其中的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。您可以选择自定义代理，让某个聊天会话采用该代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。每个练习对模式和模型的要求可能不同；相关步骤会在适用时说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选中 **Agent**，请先选择它。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单。它会显示当前启用的代理，默认值为 **default**。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    为网站添加高对比度模式。添加一个供用户切换高对比度的开关，并使用浏览器本地存储保留设置，使页面重新加载后仍然生效。
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供您评审。
6. 代理修改文件时，**Files changed** 指示器会更新，让您看到其修改的文件集合。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改仓库文件，工作时可能编辑 Astro 组件、CSS 和相关测试等文件。

下一练习中，您将评审并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域的专用 AI 助手。自定义代理可将团队的专业知识和标准整理为可重复使用的内容，引导 Copilot 更有效地完成特定类型的工作。

您了解了以下概念：

- 如何定义自定义代理。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]，在更改发生时评审，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
