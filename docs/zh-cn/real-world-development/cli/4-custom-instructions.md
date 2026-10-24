---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "浏览仓库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，就要确保 Copilot 能获取相关指导。[说明文件][instruction-files]不仅描述您需要什么代码，也说明代码应如何组织。现在已经构建了筛选功能，接下来将查看 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将：

- 了解仓库说明和路径专属说明文件如何传达给代理。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如有可注入的 `db` 参数，也应记录。
- 可重用 Astro 组件应记录其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 继续遵循现有格式和 lint 指导。

通过说明文件，可以让 Copilot 获取这些信息，并按照相关实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解您的编码风格和要求。您可以指定编码约定、库以及希望添加的注释类型等内容。说明可以应用于整个仓库，也可以只针对特定文件类型或任务。

说明文件分为两类：

- `.github/copilot-instructions.md`：Copilot 每次处理此仓库的请求时都会收到的单个说明文件。这里适合放置大多数请求都需要的项目级信息。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及支持情况会因环境而异。依赖特定格式前，请先查看[自定义说明支持参考][custom-instructions-support]。

## 浏览项目中的自定义说明文件

起始项目中已包含一组说明文件。先浏览现有文件，再进行更改，以便了解其效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些说明适用于此仓库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览其中内容。请留意 Astro 文件、Drizzle 数据层、测试等主题的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意它链接到的其他说明文件和项目现有文件。这有助于将较大的说明集拆分为可重用的小文件，并为 Copilot 提供示例。

## 根据团队指导更新说明文件

虽然现有文件已经打下良好基础，但还有一处缺失。现在修改核心 `copilot-instructions.md` 文件，确保生成新的 TypeScript 代码时包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分的最后添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在会话开始时加载仓库说明。更新说明后，退出并恢复筛选会话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选会话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存会话。
2. 在 `game-filters-cli` 分支的仓库根目录中恢复已保存的会话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选会话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚更新了说明和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并查看更改过的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

您已了解 Copilot CLI 如何读取说明文件中的上下文，并将新的标准应用到功能中。本课中，您：

- 了解仓库说明和路径专属说明文件如何传达给代理。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可重用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持参考][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
