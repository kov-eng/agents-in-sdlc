---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云端代理练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的个人副本，并启动一个 [codespace][codespaces]，用于编辑说明文件和审查云端代理生成的工作。

## 设置实验存储库

要为实验代码创建存储库副本，请从[模板][template-repository]创建一个实例。新实例将包含实验所需的全部文件，你将在后续练习中使用它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **使用此模板** 按钮，创建自己的存储库副本。然后选择 **创建新存储库**。

    ![使用此模板按钮](../../../_images/ex0-use-template.png)

3. 如果你参加的是由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下你创建的存储库路径（**organization-or-user-name/repository-name**），稍后实验中会用到。

## 创建 codespace

接下来，你将使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。等待期间，你可以探索 GitHub Copilot 的其他功能，接下来我们会介绍这些内容。

> [!CAUTION]
> 后续练习中还会用到此 codespace。现在请先保持它打开，并留在浏览器的一个选项卡中。

> [!NOTE]
> 本工作坊设计为在 codespace 或本地 [dev container][dev-containers] 中运行。这两种环境都已安装顺利完成练习所需的全部依赖项。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **在容器中重新打开**。VS Code 将构建与 codespace 使用的相同 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已创建实验存储库的副本！你也开始创建 codespace，接下来将使用它配合 Copilot 云端代理完成练习。

## 下一步

接下来为云端代理添加自定义说明。继续学习[练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [根据模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
