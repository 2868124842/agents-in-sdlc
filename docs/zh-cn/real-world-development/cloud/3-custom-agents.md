---
title: "练习 3 - 自定义代理"
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

本练习将使用自定义代理来：

- 了解如何定义自定义代理。
- 将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，一些用户觉得当前暗色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现一种可由用户切换的高对比度模式。

无障碍访问至关重要，因此需要尽快实现此功能。您将使用自定义代理生成相关功能。

## 评审无障碍自定义代理

已为您创建无障碍自定义代理。先查看其内容，了解它将如何指导 Copilot。

返回 codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含带 `name` 和 `description` 的 frontmatter。

3. 继续浏览后续部分，了解其中的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 创建并分配 issue

任务控制中心是管理环境中所有代理的中心位置。您可以在这里将任务分配给 Copilot 云智能体、监控任务，还可以重定向正在运行的会话并提供更多指导。现在，我们将任务分配给 Copilot，让它创建高对比度模式。

1. 打开您的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中突出显示了自定义代理和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域的专用 AI 助手。自定义代理可将团队的专业知识和标准整理为可重复使用的内容，引导 Copilot 更有效地完成特定类型的工作。

您了解了以下概念：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来，我们将在任务控制中心[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做准备][org-custom-agents]
- [为企业使用自定义代理做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
