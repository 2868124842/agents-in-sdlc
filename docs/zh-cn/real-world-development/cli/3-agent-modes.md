---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 协商实现方案，从 issue 出发使用 Autopilot 构建筛选功能，并在 Interactive 模式下评审结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。更大的更改则需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流程，帮助我们以正确的方式构建正确的内容。本课是后续多课的开始：您将遵循典型的代理驱动开发流程，从 issue 出发生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在整个功能工作流程中，您会使用同一段对话和同一分支。通常不同类型的文件会在不同分支或 PR 中处理；为聚焦核心概念，我们在本工作坊中简化了流程。

本课将：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现功能。
- 评审代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，您还将更新仓库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和发行商筛选游戏。待办 issue 描述了这项功能，但组合类别等细节需要在编码前达成一致。您将使用 Plan 模式敲定这些决定，然后授权 Autopilot 按明确的范围实现功能。

## 背景

在开发流程中引入 AI 编码代理并不会改变基本原则，反而会让这些原则更加重要。大多数开发人员遵循的流程大致如下：

1. 从已提交的 issue 开始，了解需要完成的工作。
2. 制定实现计划。
3. 编写并评审代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过评审且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但通常与上述流程大致相同。

遵循这一标准流程，可以确保 AI 生成的代码满足既定要求，并经过与人工编写代码相同的审核。

## 对话模式

**对话模式**决定代理拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在不同模式之间切换：

- **Interactive**：您与代理协作。代理提出更改建议，并等待您的意见再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作——编写代码、运行测试并反复迭代，直到认为任务已完成。

先使用 Plan 模式评审计划，然后使用 Autopilot 实现。

## 从 issue 开始

开始筛选功能前，返回 Codespace 并确保仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端没有打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

留意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以让您在将功能实现或任务交给 Copilot 之前，先确定具体方案。复杂任务最好先花些时间规划。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳实现方法。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答；没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何内容与您的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划编写并评审完成后，就可以开始实现。使用 Autopilot 让 Copilot 执行任务。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或所安装版本中名称类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 查看 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，所以请先评审计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 评审并验证实现

生成的代码与其他代码一样，需要在合并前评审。现在来评审代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与 issue 及规划时做出的决定进行比较。
4. 代码评审完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 体验新功能

代码看起来没问题，但能否运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中商定的组合筛选方式。
4. 确认重置和无结果时的行为符合 issue 要求和您的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

您已使用不同的对话模式来构建并评审功能。本课中，您：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现功能。
- 评审代码。
- 在转发的浏览器中手动验证功能。

接下来，了解如何生成符合已记录实践的代码：[使用自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
