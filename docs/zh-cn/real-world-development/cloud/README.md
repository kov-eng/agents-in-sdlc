---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可以让 GitHub Copilot 在云端异步工作。你在 GitHub 上分配任务后，云端代理会在后台接手工作——探索存储库、进行修改并打开拉取请求——而你可以继续处理其他事情。

在这些练习中，你将添加云端代理会遵循的自定义说明，然后分配一个 GitHub 问题并让它实现任务。你将审查和使用自定义代理来引导它的工作方式，在代理控制面板中监控并引导会话，最后审查它创建的拉取请求并迭代优化结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云端代理将遵循的自定义说明 |
| [2. 云端代理][ex2] | 异步代理 | 将问题分配给 Copilot 云端代理 |
| [3. 自定义代理][ex3] | 专用代理 | 查看并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代优化][ex5] | 审查 | 审查 PR、迭代优化 Copilot 的工作并选择后续步骤 |

## 准备工作

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云端代理

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所需的代理、MCP、代码审查和 Copilot CLI 功能，因此可使用该计划完成全部练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云端代理适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，除 Copilot Free 外的所有计划均可使用。Copilot Business 或 Enterprise 计划的组织需要由管理员启用云端代理。

> [!NOTE]
> 本练习模块不涉及 MCP。想了解如何将 MCP 服务器与 Copilot 配合使用，请查看 [CLI 练习模块](../cli/) 或 [VS Code 练习模块](../vscode/)。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
