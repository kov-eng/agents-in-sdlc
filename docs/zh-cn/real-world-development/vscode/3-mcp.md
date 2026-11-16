---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求之前，应该先确认它在浏览器中能够正常运行。你不必亲自操作应用，而是可以连接 **Playwright MCP 服务器**，让 Copilot 在真实浏览器中为你测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解什么是模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot 的能力；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式能够访问编辑器之外的资源时，它的能力会大幅增强。Copilot 通过模型上下文协议 (MCP) 实现这一点：MCP 是一种让代理与外部工具和服务通信的标准方式。

![代理模式内部工作原理及其与上下文、LLM 和工具（包括 MCP 服务器与 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理代理与外部工具之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都提供一组 AI 代理可访问的工具和资源。

下面是两个常见的 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以用它创建存储库、更新现有存储库，以及管理 issues 和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以用它打开网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，方便大家查找并为生态系统做出贡献。

> [!CAUTION]
> 对待 MCP 服务器应像对待项目中的其他依赖项一样谨慎。使用 MCP 服务器之前，请仔细审查其源代码、验证发布者，并考虑潜在的安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 配置由项目模板提供，因此上一练习中 Copilot 才能读取待办事项中的 issues。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你进行 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可让 Copilot 操控浏览器，正好适合用来测试刚完成的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列。文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器。codespace 中没有桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要访问某个服务器的工具时，VS Code 会启动该服务器，并要求你确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操控真实浏览器。你不必亲自操作应用来检查效果；代理可以打开页面、导航到相应位置、应用筛选条件，并将结果读给你听，然后总结观察结果。这是在不离开对话的情况下，快速确认功能是否符合预期的方式。

在幕后，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着代理会分析带有标签的结构化元素（按钮、链接和列表项），与辅助技术读取页面的方式相同。因此，简单的功能检查也能兼顾基本的无障碍检查。

服务器已连接且应用正在运行时，提示 Copilot 使用浏览器测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试并汇报结果。对照 issue 中的验收标准阅读总结；如果发现异常，可以继续提问，或让它在创建拉取请求前修复代码。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 上运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，包括使用 github.com 和 GitHub 命令行界面 (CLI)。不过，既然正在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关 issue，并将 PR 与 issue 关联起来。

1. 打开 Copilot Chat 面板，然后选择 **New Chat** 开始新对话。
2. 确保在代理下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你验证 GitHub 身份；按照提示完成操作。
5. 查看 Copilot 如何搜索 issues、找到相关 issue 并创建 PR。
6. 选择 Copilot 生成的链接来查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot 的能力；
- 将 Playwright MCP 服务器添加到工作区；
- 使用它在浏览器中手动测试筛选功能，然后再发布；
- 发布分支，并为筛选功能创建了拉取请求。

你已经使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，创建一个[自定义代理][next-lesson]，以便更高效地完成无障碍审查等专门任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
