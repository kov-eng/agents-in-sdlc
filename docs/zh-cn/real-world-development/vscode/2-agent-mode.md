---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件、再进行更新……还有很多类似的工作。

Copilot 代理模式正是为此而生。

Copilot 代理模式旨在 IDE 中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来，我们将了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云代理的名称相似，但它们针对的是两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云代理则像一位团队成员一样异步工作，通过 issues 和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot 代理模式探索项目、识别相关文件并协调完成更改；
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 在将 Copilot 代理模式生成的更改合并到存储库之前，审查更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和 publisher 筛选。上一练习中，你已经添加了 publisher 辅助函数；现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，用户可以发布游戏并查看游戏详情。这是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；项目没有独立的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过小型、作用范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，方便运行网站。可以按以下步骤在 GitHub Codespace 中启动：

1. 返回 codespace。上一练习结束后，你应该已经切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示一个横幅并提供 URL，内容类似下面这样：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 codespace 的专用隧道，此时 Web 服务器正在 codespace 中运行！

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择某个游戏后，会打开详情页，显示标题、说明、publisher 和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 以开始新的对话。不同练习所用的模式和模型可能不同；每个步骤会在相关处说明。

网站的初始实现已经可用，但我们希望增加新功能。先查看待办事项。你从模板设置项目时，系统已自动创建一组 issues；现在请 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。此代理会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并未偏好某个模型。编写本工作坊时，我们使用了 Claude Sonnet 4.5，因此将它列在说明中。这样做是希望你获得相对一致的代码建议和体验。不过，大型语言模型的输出具有概率性，因此建议可能与工作坊中的说明不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应该能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. 选择 **Continue** 以运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

你会看到 Copilot 根据它认为对用户最有用的程度，为这些事项排定了优先级。

## 查看说明文件

让代理生成代码前，适合先查看将用于向 Copilot 提供上下文的说明文件。接下来会使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含了如何为网站添加功能的上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍功能原则；
   - 指向不同文件类型专属说明文件的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件既可以引用其他说明文件，也可以引用项目中的文件。这些路径相对于说明文件所在位置，便于重用内容、将大型说明拆分为更易管理的小文件，以及提供供 Copilot 参考的示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用做三项单独的更新：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页面，加入筛选 UI。

此外，在将更改合并到存储库之前，必须运行并通过测试。Copilot 代理模式可以完成这些任务！现在开始添加此功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确保在 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确保仍选中模型 **Claude Sonnet 4.5**。
5. 使用待办事项中相关 issue 的要求，提示 Copilot 实现此功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与所需功能相关的文件。你应该会看到它查找数据层辅助函数、UI 和测试文件，然后开始修改这些文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和存储库规模，这可能需要几分钟。在此过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作进行中选择。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现停顿，甚至有些测试会失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 代理模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回运行着网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。我们了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 让 Copilot 代理模式探索项目、识别相关文件并协调完成更改；
- 在将 Copilot 代理模式生成的更改合并到存储库之前，审查更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 扩展探索练习 - 实现分页

随着游戏列表不断增加，需要实现分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑以下事项：

- 遵循现有最佳实践，包括使用现有说明文件；
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是将其固定；
- 编写提示时，提供必要的指导，让 Copilot 按照你的设想进行实现；
- 可能需要与 GitHub Copilot 反复交互，请它修改内容并补充上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
