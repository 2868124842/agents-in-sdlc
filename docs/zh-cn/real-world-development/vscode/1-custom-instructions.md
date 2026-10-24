---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要按特定方式完成，或者 Copilot 应了解一些背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种方式是使用[说明文件][instruction-files]，指导它如何生成代码。

本练习将：

- 了解项目专属上下文、编码指南和文档标准如何通过仓库自定义说明及路径专属说明文件传达给 Copilot。
- 在当前说明生效的情况下，生成用于筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 重新运行相同提示，观察生成代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合您设置的某些标准，因为 Copilot 的输出具有概率性。本练习旨在观察更新说明前后的**行为趋势**，而不是让输出逐字完全一致。

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

- `copilot-instructions.md` 应聚焦于项目级指导，例如项目简介、项目结构和全局编码标准。
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
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指导。这些内容都适用于您与 Copilot 的交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。请留意 Astro 文件、Drizzle 数据层、测试等主题的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于仓库根目录的 glob，用于确定说明适用的文件。此处的 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意此项目的单元测试专用说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意它链接到了其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分为可重用的小文件，并为 Copilot 提供示例。这里的路径相对于说明文件所在位置，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来，您将添加 TSDoc 文档注释和文件注释标题的规则。

## 为更改创建分支

按照最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明之前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。每个练习对模式和模型的要求可能不同；相关步骤会在适用时说明。

要查看自定义说明的影响，先在现有说明生效时发送提示。之后更新说明，再次发送相同提示并比较结果。

1. 关闭上一练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应放置的位置。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加数据访问辅助函数，返回所有发行商的列表，其中每项包含名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件及其测试。
6. 留意建议的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意建议的代码**没有** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有明确要求，它也可能添加文档注释。如果出现这种情况也没关系；本练习的重点仍是观察更新说明后行为的一致性改进。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录仓库编码标准，以改进代码建议。

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

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送**与之前相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加数据访问辅助函数，返回所有发行商的列表，其中每项包含名称和 ID。应用文件更改。
   ```

4. 留意建议的文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

您刚才引导 Copilot 遵循新的项目标准，并将其应用到下一练习将继续使用的真实代码中。

## 提交说明并推送分支

说明文件与仓库中的其他资源一样，应通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有文件更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也请暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并将其关联到待办列表中的文档标准 issue。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具完成：找到 issue、创建用于关闭该 issue 的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表您操作 GitHub。项目模板已在工作区中配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示您登录 GitHub；按提示授权即可。稍后的练习会介绍此连接如何工作（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中已选中 **Agent**。
3. 请求 Copilot 查找与说明文件更新有关的 issue，并从当前分支创建 PR，描述说明更新和新的发行商辅助函数基础：

   ```plaintext
   找到与更新说明文件有关的 issue。从当前分支创建新的 PR，说明该 PR 会关闭对应 issue，并注明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 如果 Copilot 提示您 **Allow** 它代表您执行 GitHub 操作，请检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 main：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以保持同步。
   ```

7. 如果 Copilot 提示您 **Allow** 它代表您执行 GitHub 操作和 shell 命令，请检查命令并酌情选择 **Allow**。

现在，您已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

您了解了 Copilot 如何读取此项目的说明文件上下文，并使用 VS Code 中的 Copilot Chat 完成以下操作：

- 发送代码生成提示，观察 Copilot 在**现有**说明下的输出。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 重新发送相同提示，观察建议的代码如何采用新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
