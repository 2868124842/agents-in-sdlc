---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，请先准备好所需环境。您将创建 Tailspin Toys 仓库的副本，启动一个用于开发的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置实验仓库

您将从[模板仓库][template-repository]创建自己的实例，作为编写代码的副本。新实例包含实验所需的全部文件，后续练习会用到它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面选择 **Use this template**，然后选择 **Create a new repository**，创建自己的仓库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果您参加的是由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在您有权使用 GitHub Copilot 的组织中创建新仓库。

    ![填写仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下您创建的仓库路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办列表已准备就绪**
>
> 从模板创建仓库时，系统会自动为您创建 GitHub issues 待办列表。整个工作坊中都会使用这些 issues，无需自行创建。

## 创建 codespace

接下来，您将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让您直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以先了解 GitHub Copilot 的其他功能，接下来就会介绍这些内容。

> [!CAUTION]
> 后续练习还会用到此 codespace。现在请先让它在浏览器选项卡中保持打开状态。

> [!NOTE]
> 本工作坊可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都会安装实验所需的全部前置组件，确保流程顺畅。如果您希望在本地运行，请在 VS Code 中打开克隆的仓库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

若要使用 GitHub Copilot Chat 代理模式，IDE 中需要安装 GitHub Copilot Chat 扩展。如果您使用 GitHub Codespaces，通常已预装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开“扩展”视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，您可能需要使用 GitHub 帐户进行身份验证。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息并按 Enter。这应该会激活 Copilot Chat。
4. 如果尚未进行身份验证，系统会提示您登录 GitHub 帐户。按照提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会出现。

## 总结

恭喜，您已创建实验仓库的副本，并开始创建 codespace。开始编写代码时会使用这个环境。

## 下一步

现在开始让 Copilot 协助工作。继续学习[练习 1：自定义说明][next-lesson]，了解如何让 Copilot 遵循项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
