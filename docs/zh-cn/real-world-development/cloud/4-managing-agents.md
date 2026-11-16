---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot 云端代理处理了三项任务，重点是改善用户体验和添加功能。云端代理的设计目标是异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云端代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心中，可以查看所有包含未关闭拉取请求（PR）的代理任务、探索代理执行的操作，还可以引导正在运行的会话。

本课将学习如何：

- 使用代理页面监控云端代理任务。
- 引导正在运行的会话，要求添加额外功能。

## 场景

分配高对比度模式任务后，团队意识到同时添加浅色模式也是合适的。网站样式和切换功能都在更新，因此加入浅色模式似乎很合理。现在，你将引导代理，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云端代理任务

查看当前分配给 Copilot 云端代理的所有任务的状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务的各种操作。

> [!NOTE]
> 显示的确切步骤会根据 Copilot 当前的工作状态和采用的方法而变化。

4. 还要查看右侧显示的拉取请求（PR）面板。你可以在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云端代理

你已经了解当前正在执行的任务。现在让 Copilot 在实现高对比度模式的同时，也添加浅色模式切换功能。

1. 选择与添加高对比度模式有关的会话。确切标题可能因 Copilot 采用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始工作。出现类似以下消息时，表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面上的云端代理任务截图，突出显示引导活动会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，然后将新请求加入工作流程。

这同样需要几分钟。此时可以暂停一下，回顾到目前为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云端代理任务的中心位置。你可以通过任务控制中心跟踪所有正在进行和已完成的任务、审查已执行的工作，还可以调整正在运行任务的范围或提供额外指导。

你探索了以下概念：

- 使用任务控制中心和代理页面监控云端代理任务。
- 重定向正在运行的会话，请求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
