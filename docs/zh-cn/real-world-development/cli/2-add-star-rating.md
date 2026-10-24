---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 小幅修改游戏卡片，在转发的浏览器中评审更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以对项目进行第一次更改了。先从小处着手：游戏数据中已有星级评分，但首页上的游戏卡片还没有显示。您将让代理显示评分、评审更改，并将其作为第一个拉取请求合并。

本课将学习如何：

- 在功能分支上开始一段专注的 Copilot 对话。
- 请求代理对项目进行小幅修改。
- 使用 `/diff` 评审更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每个游戏都可以有星级评分，而且游戏详情页已经显示评分。不过，首页游戏卡片只显示标题、类别、发行商和简介。作为热身，您将让代理在每张卡片上显示已有评分。这项更改很小且范围明确，适合作为第一次会话任务。

## 对话的组成

**对话**是您使用 Copilot CLI 处理任务的空间。与 Copilot 应用不同，常规 CLI 对话使用终端当前检出的仓库和 Git 分支，而不会创建专用工作树。保存的对话可以让您稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三项内容：您的提示和代理回复；代理探索和编辑文件时的工具活动；以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

现在开始新对话，着手实现功能。

1. 返回 Codespace。
2. 如果终端没有打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的每张游戏卡片上显示游戏的 5 分制星级评分 `starRating`。如果评分为 null，则显示“No rating yet”。保持卡片布局不变，添加测试，并运行相关检查。
   ```

Copilot 会探索项目、找到显示游戏详情的文件并创建所需代码。您已使用 Copilot CLI 添加了一项新功能。

## 评审差异

所有 AI 生成的更改都应在合并前评审，即使更改很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查所有已更改文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试涵盖这两种情况。
4. 查看 Copilot 运行检查的结果，并要求它修复任何失败项。
5. 评审完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 与所有生成式 AI 工具一样，输出具有概率性而非确定性，因此实际代码可能有所不同。请评审行为，而不要期待实现方式完全一致。

## 检查更改

不要只读代码就假设它能正常运行。让 Copilot 启动网站，在 Codespaces 转发的浏览器中查看更新后的用户界面（UI）。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示五分制评分。
4. 返回 Copilot，并要求它停止刚才启动的服务器：

   ```plaintext
   停止你刚启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已经实现，现在创建拉取请求（PR），将新代码合并到项目中。

1. 请求默认代理提交更改：

   ```plaintext
   使用恰当的提交消息提交已评审的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 评审已更改文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜，您已使用 GitHub Copilot CLI 发布第一个更改。具体而言，您：

- 在功能分支上开始了专注的 Copilot 对话。
- 指示代理对游戏卡片进行小幅修改。
- 使用 `/diff` 评审了更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选 issue 入手，使用 Plan 和 Autopilot 模式构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
