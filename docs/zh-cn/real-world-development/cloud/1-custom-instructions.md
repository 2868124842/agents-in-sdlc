---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要按特定方式完成，或者 Copilot 应了解一些背景信息，就要确保它能访问这些上下文。[说明文件][instruction-files]可提供这些指导，让 Copilot 不仅了解您要完成的任务，也了解应如何完成。

本练习将：

- 了解项目专属上下文、编码指南和文档标准如何通过仓库自定义说明及路径专属说明文件传达给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，本练习不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub issues，因此很难实时并排展示效果。稍后评审云智能体生成的拉取请求时，您会看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应使用 TSDoc 文档注释记录代码。
- 每个文件开头都应有一段注释，说明文件的用途。

通过说明文件，可以让 Copilot 获取这些信息，并按照相关实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解您的编码风格和要求。您可以指定编码约定、库以及希望添加的注释类型等内容。说明可以应用于整个仓库，也可以只针对特定文件类型或任务。

说明文件分为两类：

- `.github/copilot-instructions.md`：Copilot 每次处理此仓库的请求时都会收到的单个说明文件。这里适合放置大多数聊天或 CLI 请求都需要的项目级上下文，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 中的代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体会使用仓库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持使用其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md。[查看支持的说明文件格式][custom-instructions-support]。

### 管理说明文件的最佳实践

全面介绍如何创建说明文件超出了本工作坊的范围。示例项目展示了一种具有代表性的做法。总体而言：

- `copilot-instructions.md` 应聚焦于项目级指导，例如项目结构、开发内容和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，让指导清楚明了，并给出代码应该和不应该采用的形式示例。

说明文件没有唯一正确的写法，AI 的使用方式也不止一种。通过实践，您可以找到适合项目的方案。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的说明文件。在浏览此项目的文件时，您会看到针对多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关课程会在适用时介绍具体操作。
>
> 需要模板或起点？请探索包含说明文件、自定义代理和其他资源的 [awesome-copilot][awesome-copilot] 仓库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义说明文件

花些时间阅读此仓库中的说明文件：一个核心 `copilot-instructions.md` 文件，以及针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。使用 Copilot 时，这些指导都会生效。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。请留意 Astro 文件、Drizzle 数据层、测试等主题的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于仓库根目录的 glob，用于确定说明适用的文件。此处的 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意此项目的单元测试专用说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意它链接到了其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分为可重用的小文件，并为 Copilot 提供示例。文件中的路径相对于说明文件所在位置，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来，您将添加 TSDoc 文档注释和文件级注释的规则。

## 添加新的仓库标准

接下来，您将进行本练习中的少量编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释。云智能体稍后处理分配给它的 issue 时会采用这条规则。

编辑前，请先创建一个分支（云智能体会从 issue 目标分支读取说明，但仍建议将更改整洁地提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在，让我们记录编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行附近）。留意其中记录了项目的编码标准，但尚未规定代码文档要求，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在仓库级别（`.github/copilot-instructions.md`）创建说明文件来提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。对于 TSDoc 注释这类项目级标准，仓库级文件最合适。

## 提交、推送并合并说明文件更改

云智能体会从 issue 目标分支读取说明。下一练习中，您会从 `main` 将 issue 分配给 Copilot，因此必须先将说明更改合并到 `main`，云智能体才能使用它。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中分配的 issue 将由云智能体读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成本工作坊，可以跳过分支步骤，直接提交到 `main`。创建分支是为了模拟真实项目中的常规做法。

## 总结和后续步骤

您已了解 Copilot 如何获取此项目说明文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的仓库级标准。接下来的练习中，您会看到云智能体生成的拉取请求如何遵循该标准。

接下来，[将第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
