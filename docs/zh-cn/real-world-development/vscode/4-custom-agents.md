---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义代理，为 Copilot 提供有针对性的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可重复使用的代理，例如负责检查 [WCAG][wcag] 合规性的无障碍代理、遵循安全编码实践的安全代理，或确保测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，后面接一段定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理与[代理技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，用于指导 AI 执行操作。最简单的区别是：**自定义代理**是执行工作的人员，**技能**是工具。

自定义代理拥有自己的上下文窗口，能够协调技能（甚至其他代理）来完成任务。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他用法一样，需要根据自己的环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文字与背景颜色对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。接下来将使用自定义代理生成此功能。

在本练习中，你将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已经为你创建了一个无障碍自定义代理。返回 codespace，然后打开终端，为无障碍功能创建一个基于 `main` 的新分支（筛选功能的 PR 会与此工作保持分离）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` 是自定义代理 frontmatter 中的必需字段。

3. 接着浏览并查看后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 定义的所有自定义代理。选择自定义代理后，可以让当前聊天会话遵循该代理的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 以开始新的对话。不同练习所用的模式和模型可能不同；每个步骤会在相关处说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（它会显示当前代理；默认是 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始处理任务：探索存储库、提出更改建议，并将编辑应用到项目文件。每项编辑都会以内联方式显示在聊天中，其中包括文件路径和可供审查的差异。
6. 代理修改项目文件后，**Files changed** 指示器会更新，以显示已更改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习会介绍如何审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 自定义代理][custom-agents]，也就是专门针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队的专业知识和标准编写成可重复使用的代理，引导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 自定义代理的定义方式；
- 在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理工作][next-lesson]，在同一个会话中审查更改，并添加浅色模式切换开关。

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
