---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "浏览现有 quality-checks 技能、自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证了代码功能，并使用说明文件确保它符合标准。但测试、lint 和持续集成（CI）的其他部分又该如何处理？

这类任务非常适合使用**代理技能**。技能可以帮助 Copilot 正确执行这些操作。

本课将：

- 浏览现有的 `quality-checks` 技能。
- 自定义其结果格式。
- 重新加载并运行此技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

代理技能会打包可重复使用的任务说明、可执行脚本和支持资源，供代理按需加载。技能的基本结构是一个与技能同名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件通过 frontmatter 中的名称和说明定义技能，介绍其作用并说明何时应调用。文件夹还可以包含供技能使用的脚本及其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和重用的仓库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

我们来查看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明内容可帮助 Copilot 判断何时调用此技能。
3. 阅读说明，留意它如何引导 Copilot 执行测试和 lint。
4. 注意该技能目前没有 **Results output formatting** 部分。

## 更改前先运行技能

可以通过 Copilot CLI 直接调用技能，也可以使用自然语言请求。现在让 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式下的筛选会话。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望改进报告，让它说明实际运行了哪些命令、是否成功以及工具报告了什么。现在更新技能，生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在看看更新后的技能如何工作。Copilot CLI 可以在不重启会话的情况下重新加载编辑过的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 留意最后生成的报告，并与第一次报告比较。

## 总结和后续步骤

您已自定义并使用了现有代理技能。本课中，您：

- 浏览了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能的结果格式。
- 重新加载并运行了技能。

这项更改会与筛选功能一起包含在功能 PR 中。接下来，让 Copilot [通过 Playwright MCP 服务器直接与网站交互][next-lesson]。

## 更多技能示例

以下社区示例供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
